# PhotoMap - Your Map-Based Photography Archive

## 1. Description
PhotoMap: Personalized map-based photography assistant and archive. Users can upload photos and have them displayed on the map, at the same location where the photo was taken. Uses can view their uploaded photos and navigate between their location markers on the map. PhotoMap will also provide recommendation information about potential locations of interest, based on the geographical data of location-markers and user's photo history. 


## 2. Specifications

### **2.1. Actors Description**
1. **Indie photographers (User)**: take photos and update locations with photos; receive recommendation of places of interests, as well as sharing their photos for other photos. 

### **2.2. Functional Requirements**
<a name="fr1"></a>

1. **Log In** 
    - **Overview**:
        1. Create Account
    
    - **Detailed Flow for Each Independent Scenario**: 
        1. **Create an Account**:
            - **Description**: The user creates a new account in the system by Google sign-in. The Google Sign-In API handles existing user log-in and creates new google users. 
            - **Primary actor(s)**: User
            - **Main success scenario**:
                - 1.The user navigates to the “Sign Up” page.
                - 2.The user is taken to the Google Sign-in API.
                - 3.The system confirms successful account creation and provides login prompts.
            - **Failure scenario(s)**:
                - 2a. Google Sign-in API returns Error
                    - 2a1. The system displays an error message prompting the user to try sign-in again.
                - 3a. The system cannot store the user data due to a server error or network issue.
                    - 3a1. The system displays an error message and prompts the user to try again later.

2. **Upload Photos** 
    - **Overview**:
        1. Create Marker
        2. Upload Photos
    
    - **Detailed Flow for Each Independent Scenario**:

        1. **Create Marker**:
            - **Description**: The user creates a marker, pinpointing a location  on the map. They can use this location marker to upload and view their photos
            - **Primary actor(s)**: User
            - **Main success scenario**:
                - 1.The user clicks on any non-marker-occupied location on the map. A form appears prompting the user to fill in marker info.
                - 2.The user fills in the marker title and selects a marker color from the form, and clicks "Add" to confirm.
                - 3.The marker is created and saved to the database. A message appears notifying the user that the marker is successfully added. 
                - 4.The camera moves to center on the newly created marker. 
            - **Failure scenario(s)**:
                - 3a. The user changes their mind and clicks cancel to abort marker creation
                    - 3a1. The bottomsheet disappears and no marker is shown on the map.  

        2. **Upload Photos**:
            - **Description**: The user uploads photos from their local device to the system by clicking "Upload Photos". 
            - **Primary actor(s)**: User
            - **Main success scenario**:
                - 1.The user selects a marker and the upload photo button appears. It should not appear if a user does not have a button selected. 
                - 2.The user clicks the upload button, and a bottomsheet appears prompting the user to select a photo from their device.
                - 3.The user selects a photo from their device and clicks submit. 
                - 4.The system displays a success message and shows the newly uploaded photos in a recyclerview update above the marker.
            - **Failure scenario(s)**:
                - 3a. The user tries to upload a null image.
                    - 3a1. The system outputs a message: No image selected. 
                - 4a. Network or server error: the upload fails due to a connection issue.
                    - 4a1. The system notifies the user and offers the option to retry or cancel.

3. **View Map Info** 
    - **Overview**:
        1. View Map Info
    
    - **Detailed Flow for Each Independent Scenario**: 
        1. **View Map Info**:
            - **Description**: The user can pan around the map to see their markers and click on markers to see previously uploaded photos. 
            - **Primary actor(s)**: User
            - **Main success scenario**:
                - 1.The user navigates to the “Map” section.
                - 2.The system loads the map interface with the user’s current location and saved locations.
                - 3.The user can zoom in/out or pan around the map.
                - 4.The user selects a specific location or marker to view more details (e.g., marker name, photo preview, option to delete marker).
            - **Failure scenario(s)**:
                - 2a. The system fails to retrieve marker or photo data from the backend.
                    - 2a1. An error message is displayed indicating that marker and photo data fetching has failed. 

4. **Receive Location Recommendation** 
    - **Overview**:
        1. Receive Location Recommendation
    
    - **Detailed Flow for Each Independent Scenario**: 
        1. **Receive Location Recommendation**:
            - **Description**: The system provides location recommendations based on previously uploaded photos and marker density. Once the "Recommend" button is clicked, the user receives a summary of their major photo themes and most active locations. The recommendation also includes a list of locations matching the user's preferences.
            - **Primary actor(s)**: User
            - **Main success scenario**:
                - 1.The user clicks the button "Popular Location Notification"
                - 2.The system displays a bottomsheet with the user’s most active location and relevant tags. The bottomsheet also contains a list of recommended locations. 
                - 3.The user selects a recommended location and the system creates a marker at the selected location. 
            - **Failure scenario(s)**:
                - 2a1.  User uploads no images or creates no markers before clicking "Recommend".
                    - 2a1.1. The system displays a message prompting the user to create more markers and upload more images. 
                - 2a2. No relevant recommendations are available
                    - 2a2.1. The system notifies the user that there are no relevant locations that match the user’s preferences. 
                - 4a. The system encounters a server error or network issue and cannot fetch recommendations.
                    - 4a1. The system displays an error message and prompts the user to retry later.
    
5. **View Gallery** 
    - **Overview**:
        1. View and Share Images inside Gallery
    
    - **Detailed Flow for Each Independent Scenario**: 
        1. **Share a Photo from Gallery**:
            - **Description**: The user goes to the photo gallery and chooses  to share  photos (one photo at a time) to the other user of the app.
            - **Primary actor(s)**: User
            - **Main success scenario**:
                - 1.The user navigates to “Gallery” and selects a picture for sharing.
                - 2.The user clicks the image that they want to share and goes to full screen view mode.
                - 3.The user click the share button on the bottom right corner below the image.
                - 4.The user can either manually enter the user email of the people they want to share to or click the dropdown menu to select the person if he/she is already the user's friend.
                - 5.Users click the button on the pop up window with the text “share” to share the image.
                - 6.System confirms the image has been shared.
            - **Failure scenario(s)**:
                - 3a. If the image is shared by someone else to you, then you don’t have permission to share this image and thus the share button will not appear.
                - 6a. The system notifies the email that the user wants to share the image to is either invalid or not a user of this app.
                - 6b. The system encounters a server error or network issue and cannot process the sharing request from the user.


## 3. Frameworks
1. **AWS EC2 Instance**
    - **Purpose**: Provide a online cloud for database connection between backend and frontend
    - **Reason**: It is a powerful cloud provider that can provide a stable connection cloud server
2. **AWS S3 Cloud Storage**
    - **Purpose**: Provide an online cloud data storage for image storage and is mandatory in order to use some external modules
    - **Reason**: MongoDB and SQL-like databases are not ideal for storing images. Alternatively, we will use AWS Rekognition as our model to detect uploaded images is appropriate, and it is required to use AWS S3
3. **AWS Route 53**
    - **Purpose**: Provide an domain to our EC2 instance
    - **Reason**: In case to use SSL certification. We need to have a stable domain and record our aws public DNS. Otherwise, we cannot use https to access our routes. 


## 4. Contributions

**Jiashu Long**
- Google Map Marker Management (Storage, Display, Organization)
- Photo Upload Pipeline: Frontend UI and Image Pre-processing
- Recommendation Pipeline, using Marker data and User images for smart recommendation. 
- App Tutorial


**Alex Cheng**
- Photo Upload Pipeline: AWS EC2 Image storage 
- Data post-processing and storage.
- Backend APIs
- KNN Recommendation logic


**Ray Yu**:
- Frontend Photo Album feature
- Google Login functionality






    