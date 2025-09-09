https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly

# -----
# P1
# Functional Requirements

The first thing you want to do when starting a system design interview is 
to get a clear understanding of the requirements of the system. 

Functional requirements are the features that the system must have to satisfy the needs of the user.

In some interviews, 
the interviewer will provide you with the core functional requirements upfront.

In other cases, 
you'll need to determine these requirements yourself. 

If you're familiar with the product, 
this task should be relatively straightforward. 

However, if you're not, 
it's advisable to ask your interviewer some clarifying questions to gain a better understanding of the system.

The most important thing is that you focus on the top 3-4 features of the system 
and don't get distracted by the bells and whistles.

We'll concentrate on the following set of functional requirements:
Core Requirements
    Users should be able to submit a long URL and receive a shortened version.
        Optionally, users should be able to specify a custom alias for their shortened URL.
        Optionally, users should be able to specify an expiration date for their shortened URL.
    Users should be able to access the original URL by using the shortened URL.

Below the line (out of scope):
    User authentication and account management.
    Analytics on link clicks (e.g., click counts, geographic data).


# -----
# P2
# Non-Functional Requirements

Next up, 
you'll want to outline the core non-functional requirements of the system. 

Non-functional requirements refer to specifications about 
how a system operates, 
rather than what tasks it performs. 

These requirements are critical as they define system attributes like 
scalability, latency, security, and availability, 
and are often framed as specific benchmarks
—such as a system's ability to handle 100 million daily active users or respond to queries within 200 milliseconds.

Core Requirements
The system should ensure uniqueness for the short codes (no two long URLs can map to the same short URL)
The redirection should occur with minimal delay (< 100ms)
The system should be reliable and available 99.99% of the time (availability > consistency)
The system should scale to support 1B shortened URLs and 100M DAU

Below the line (out of scope):
Data consistency in real-time analytics.
Advanced security features like spam detection and malicious URL filtering.

# -----
# Defining the Core Entities

We recommend that you start with a broad overview of the primary entities. 

At this stage, it is not necessary to know every specific column or detail. 
We will focus on the details, such as columns and fields, later when we have a clearer grasp. 

Initially, establishing these key entities will guide our thought process and lay a solid foundation as we progress towards defining the API.

Just make sure that you let your interviewer know your plan so you're on the same page. 

I'll often explain that I'm going to start with just a simple list, 
but as we get to the high-level design, 
I'll document the data model more thoroughly.

In a URL shortener, 
the core entities are very straightforward:

Original URL:   The original long URL that the user wants to shorten.
Short URL:      The shortened URL that the user receives and can share.
User:           Represents the user who created the shortened URL.

In the actual interview, 
this can be as simple as a short list like this. 

Just make sure you talk through the entities with your interviewer to ensure you are on the same page.

# -----
# The API

The next step in the delivery framework 
is to define the APIs of the system. 

This sets up a contract between the client and the server, 
and it's the first point of reference for the high-level design.

Your goal is to simply go one-by-one through the core requirements and define the APIs that are necessary to satisfy them. 

Usually, these map 1:1 to the functional requirements, 
but there are times when multiple endpoints are needed to satisfy an individual functional requirement.

9/10    you'll use a REST API and focus on choosing the right HTTP method or verb to use.
POST:   Create a new resource
GET:    Read an existing resource
PUT:    Update an existing resource
DELETE: Delete an existing resource

To shorten a URL, 
we'll need a POST endpoint that takes in the long URL and optionally a custom alias and expiration date, 
and returns the shortened URL. 

We use post here because we are creating a new entry in our database 
mapping the long url to the newly created short url.

// Shorten a URL
POST /urls
{
  "long_url": "https://www.example.com/some/very/long/url",
  "custom_alias": "optional_custom_alias",
  "expiration_date": "optional_expiration_date"
}
->
{
  "short_url": "http://short.ly/abc123"
}

For redirection, 
we'll need a GET endpoint that takes in the short code and redirects the user to the original long URL. 

GET is the right verb here because we are reading the existing long url from our database based on the short code.

// Redirect to Original URL
GET /{short_code}
-> HTTP 302 Redirect to the original long URL

# -----
# High-Level Design

We'll start our design by going one-by-one through our functional requirements and designing a single system to satisfy them. 
Once we have this in place, we'll layer on depth via our deep dives.

1) Users should be able to submit a long URL and receive a shortened version
The first thing we need to consider when designing this system is how we're going to generate a short url. 
Users are going to come to us with long urls and expect us to shrink them down to a manageable size.
We'll outline the core components necessary to make this happen at a high-level.

Create a short url

Client  <------------>           Primary Server      <------>    Database
         POST /urls          1. Generate short url               Urls
                             2. Save to db                          - short url code (or custom alias)
                                                                    - original url   
                                                                    - creationTime
                                                                    - expirationTime
                                                                    - createdBy

Client:             Users interact with the system through a web or mobile application.
Primary Server:     The primary server receives requests from the client and handles all business logic like short url creation and validation.
Database:           Stores the mapping of short codes to long urls, as well as user-generated aliases and expiration dates.

When a user submits a long url, 
the client sends a POST request to /urls with the long url, custom alias, and expiration date. 
Then:
The Primary Server receives the request and validates the long URL.
This is to ensure that the URL is valid (there's no point in shortening an invalid URL) 
and that it doesn't already exist in our system (we don't want collisions).
To validate that the URL is valid, 
we can use popular open-source libraries like is-url or write our own simple validation.
To check if the URL already exists in our system, 
we can query our database to see if the long URL is already present.
If the URL is valid and doesn't already exist, 
we can proceed to generate a short URL 
For now, we'll abstract this away as some magic function that takes in the long URL and returns a short URL. 
We'll dive deep into how to generate short URLs in the next section.
If the user has specified a custom alias, 
we can use that as the short code (after validating that it doesn't already exist).
Once we have the short URL, 
we can proceed to insert it into our database, storing the short code (or custom alias), long URL, and expiration date.
Finally, we can return the short URL to the client.

2) Users should be able to access the original URL by using the shortened URL
Now our short URL is live and users can access the original URL by using the shortened URL.
Importantly, this shortened URL exists at a domain that we own! 
For example, if our site is located at short.ly, 
then our short urls look like short.ly/abc123 and all requests to that short url go to our Primary Server.

Redirect to original url

                            Write
                            1. Generate short url
                            2. Save to db

Client      <----------->       Primary Server      <------------>      Database

        POST /urls                                                           Urls
        GET  /{shortCode}                                                       - short url code  (or custom alias)
                                                                                - original url
                            Read                                                - creationTime
                            1. Look up original url in db                       - expirationTime
                            2. Return with 302 redirect                         - createdBy

When a user accesses a shortened URL, the following process occurs:
The user's browser sends a GET request to our server with the short code (e.g., GET /abc123).
Our Primary Server receives this request and looks up the short code (abc123) in the database.
If the short code is found and hasn't expired (by comparing the current date to the expiration date in the database), the server retrieves the corresponding long URL.
The server then sends an HTTP redirect response to the user's browser, instructing it to navigate to the original long URL.

There are two main types of HTTP redirects that we could use for this purpose:
301 (Permanent Redirect):
    This indicates that the resource has been permanently moved to the target URL. 
    Browsers typically cache this response, meaning subsequent requests for the same short URL might go directly to the long URL, bypassing our server.
    The response back to the client looks like this:
    HTTP/1.1 301 Moved Permanently
    Location: https://www.original-long-url.com
302 (Temporary Redirect): 
    This suggests that the resource is temporarily located at a different URL. 
    Browsers do not cache this response, ensuring that future requests for the short URL will always go through our server first.
    The response back to the client looks like this:
    HTTP/1.1 302 Found
    Location: https://www.original-long-url.com

In either case, 
the user's browser (the client) will automatically follow the redirect to the original long URL and users will never even know that a redirect happened.
For a URL shortener, 
a 302 redirect is often preferred because:
It gives us more control over the redirection process, allowing us to update or expire links as needed.
It prevents browsers from caching the redirect, which could cause issues if we need to change or delete the short URL in the future.
It allows us to track click statistics for each short URL (even though this is out of scope for this design).

# -----


# -----
# Q P1

refer my last 3 inputs 
prepare short answer quiz 
Come up with questions from sentence 1 to last sentence,
the answer for the question should force me to repeat the sentence


# **Functional Requirements – Short Answer Quiz**

1. What is the first thing you should do when starting a system design interview?

2. How are functional requirements defined?

3. When might an interviewer provide the core functional requirements upfront?

4. What should you do if the interviewer doesn’t provide the requirements?

5. If you are familiar with the product, how easy is it to determine the functional requirements?

6. What is advisable if you are not familiar with the product?

7. What is the most important thing to focus on regarding features during system design?

8. Which functional requirements will we concentrate on?

9. What is the core requirement regarding submitting a long URL?

10. What is an optional feature regarding custom aliases?

11. What is an optional feature regarding expiration dates?

12. What should users be able to do with the shortened URL?

13. What are examples of “below the line” or out-of-scope features?

---

If you want, I can **also make an answer key** where each answer is the **exact sentence from your text**, so you can practice repeating them word-for-word.

Do you want me to do that next?
