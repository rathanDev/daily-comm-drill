
# ---------------------------------

1. Designed and implemented the email service 
    to avoid 504 timeouts 
        bcz when data is saved the backend peii-compass downloads the attachment and sends the emails 
    to decouple email template logic 
    to decouple 
    to reduce the load for CodeQL (bcz it was running for > 30 mins)
    to improve the testing 

1. Redesigned the file upload feature and reduced the round trip time by 30%.

2. Enhanced observability, and reduced the troubleshooting time for bugs by 40%.
    Added logs 
    Added span id, trace id
    introduced Serilog 

    Used SSE (Server Sent Events) to notify the UI if the email sending successful or not.

3. Designed and lead the development of a url shortener feature
        bcz there are are many features use link sharing 
        in emails and notifications
        there were reported defects of broken urls due to special chars 

# ---------------------------------


let's stick with S3
bcz sharepoint has limitations

Mention 
that previously 

/save
    mutation api 
    saves data of trigger 

/upload 
    saves file 

/save 
    mutation api 
    updates status 
    send email 


UI ---->  BE -----> S3





I redisigned the flow 

/save 
    req
        header 
            jwt - userId
        body
        {
            data: {..., fileName, fileType, contentType}
        }
    res
        {
            presignedUrl
        }
    ;
        saves data 
        creates presigned url 
        returns presigned url
    ;
    UI ---> BE 


/upload 
    req
    {
        url: presignedUrl 
        file: file
    } 
    res 
    {
        status
    }
    ;
    client uses presigned url to upload the file directly to blob storage 
    predesigned URL expires after 5-10 mins
    PUT only allowed
    ;
    UI ---> sharePoint


    
Sharepoint trigger 
        once file is uploaded, 
        1. sharepoint calls BE API 
            1.1. update the status to "successfully uploaded"
            2.2. sends email 
            1.3. push notification to front end 
