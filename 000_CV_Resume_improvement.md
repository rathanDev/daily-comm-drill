
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


UI ---->  BE -----> sharePoint 





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
