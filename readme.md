# **Fully Automated Poster Shop**

This is a large automation flow that is capable of fully running and maintaining a poster shop without any human intervention, from the inception of the design, to the listing in the online store, posting on Social media, and even creating and shipping the product to the customer. 

### 🤖 External API Integrations Used:
- ChatGPT
- Google Suite (Drive, Sheets)
- Mallabe Images
- Everypixel
- Printify
- Etsy
- ImgBB
- Cloudconvert
- Dynamic Mockups
- Social Media (Instagram, Pinterest, Deviantart)
- Stable Diffusion

### ✨ Features:
- Does everything you need to run an ecommerce poster shop
- Uses Generative AI to create designs, and integrates AI QA to filter out bad image.
- Randomizes the prompts to ensure you never get the same image twice.
- Automatically creates mockup images and includes image watermarking.

### 🌊 The Flow:
1. **Design Generation.json**
   - Creates the image.
2. **Image Recorder.json**
   - Records images for future steps.
3. **Image Processing.json**
   - Prepares the image and gathers any necessary information.
4. **Publish Listing.json**
   - Creates the listing on Printify and Etsy.
5. **Listing Images.json**
   - Creates the mockups and attaches all images to the listing.
6. **Socials Posting.json**
   - Posts the images to Social Media
  
# Pipeline Breakdowns:
_Please note that in interest of space, the pipelines have been compressed to fit a single screen_
      
### 1. (Design Generation.json) 
![Design Generation](https://github.com/user-attachments/assets/19f396cc-370d-41b3-a38f-0e1c252949b0)

 - The workflow is split into a few different areas:
    - The left-top side is the "admin area. It has three paths. The first and second are for manual QA. While the automated system did "work" somewhat, it wasn't perfect. As a result I wanted the capability to perform manual QA. The automation performs clean up as needed and pushes manual passes through to the next stage. The third path leads to the main pipeline and allows the user to specify factors such as how many images to generate and their API keys.
    - The bottom section is the largest, and is more substantial than is necessary. It utilizes the included excel document (Art Themes.xlsx), and random number generation to create different images based on the daily theme. The reason behind the size of this section is that there are seven similar but different paths depending on which week day it is, and some are more involved than others. When the random prompt has been generated, it's put through ChatGPT to enhance the prompt to something more tailored for Stable Diffusion.
    - The next step in the pipeline is to finish tailoring the prompts. It utilizes a repeater and an increment function to generate each prompt one by one until all five are completed. The initial generations are then run through Everypixel to try and filter out any images we wouldn't want to waste credits upscaling. When the initial generations are complete, the final step is to upscale the images and run them through QA again, this time with a stricter quality requirement. The images are saved on Drive and Sheets, in order to be worked on in future steps. (_Please note that the QA filtering is disabled by default. It still runs QA to record the info but all images generated will automatically be passed._)

- If I was to rebuild this automation now, I would use a webhook to trigger the next step, redesign the image generation, (_likely away from the official Stable Diffusion API_), refine the QA, and reduce the complexity of the prompt generation, ideally "pre-generating" multiple prompt options and just pulling them from a database, versus doing it on the spot within this flow. 
    

      

### 2. (Image Recorder.json)
![Image Recorder](https://github.com/user-attachments/assets/9cff006f-424e-4fa8-82d3-a8fb7beec33f)

- This is a simple scenario that checks the drive folder for any files and simply records them. This allows me to not only use the automation, but to add in additional art as well. As long as it's in the drive folder it will be included.

### 3. (Image Processing.json)
![Image Processing](https://github.com/user-attachments/assets/4c8c841a-7108-42b7-8664-dade390dab3f)

- This workflow resizes the file if it's too large to be posted in future steps. Additionally, it uses ChatGPT to give the piece a title, description, and social media caption.



### 4. (Publish Listing.json)
![Publish Listing](https://github.com/user-attachments/assets/c4878e2f-e698-472f-91a6-528952b155e1)


- This scenario is designed to publish our new art piece on Printify. To do this, I had to develop a creative solution to properly price each product while also including everything required on the listing without manual intervention. to do this the automation takes a preset profit % and creates a dummy listing in order to grab the correct production costs. It then calculates the price given store fees, costs, and the desired profit.
- The reason for the relatively convoluted solution is due to how the Printify API is set up. As of the time of building there was no way to get production and shipping costs without creating the listing first. As a result, it's impossible to know what to set the price as. By creating a dummy listing we can grab that information, and then recreate the listing with the correct values input.
- The top right of the pipeline is split into four paths depending on the aspect ratio. The options I chose were pre-determined, and it could be expanded to take on additional sizes as well. However, as our solution requires us to generate the printify costs for each size, four sizes felt sufficient here. 
- By creating and deactivating the Etsy listing, it allows us to finish the "admin" side of the printify creation, and allows us to just modify everything as desired in the next step.

**Another pipeline that is important to mention here is the (Printify Info Checker.json):**
![Printify Info Checker](https://github.com/user-attachments/assets/c2a296c8-c673-411d-92c2-c7692cac387d)

- This automation grabs our Printify info beforehand. By default it checks the lowest cost producer for each size, and saves the different necessary variables in a spreadsheet to be referenced later. This isn't neccessarily a part of the pipeline, but rather something that could be run periodically to ensure the values are up todate.

### 5. (Listing Images.json)
![Listing Images](https://github.com/user-attachments/assets/945eedb3-e67e-4423-8580-fec94e7353e6)

- While it's a larger automation, it's actually fairly simple in nature. The first step is to generate different mockup images for the listing. In it's current form, it generates 8 options:
   1. The highres image with a watermark
   2. The second and third are room shots from Printify. Technically the second would be from Dynamic Mockups, but that is currently disabled on the flow.
   3. The next four are the various cuts depending on the image size (due to cropping from different aspect ratios) which we pull directly from Printify.
   
![chrome_k0gc6As6mQ](https://github.com/user-attachments/assets/9031c944-6347-42c4-bc8c-ca7756f4f919)
![chrome_qeef8JcGEN](https://github.com/user-attachments/assets/c0aa72da-8172-41d0-be04-71b46ce486c4)
![chrome_1YqwHhyl0B](https://github.com/user-attachments/assets/d53f2690-2cea-483d-aae0-051abdaee805)


### 6. (Socials Posting.json)
![Socials Posting](https://github.com/user-attachments/assets/901843a7-f99a-4389-9ef4-5427a2ee08a7)

- The final step in the pipeline is to post the art on Social Media. It's set up to post on Instagram, Pinterest, and Deviant art.
- The posting data is also stored in a Spreadsheet for automated analysis. 

   ![image](https://github.com/user-attachments/assets/5f66d88c-ee99-46bb-a84a-765767b8109a)

**As a bonus, the Instagram Reporting Automation:**
![Daily Instagram Reporting](https://github.com/user-attachments/assets/b9207b37-9db1-4368-9d6a-71e97742b420)

- This automation is designed to grab daily, seven day, and 30 day stats from our Instagram content. This data is recorded on various sheets which can then be used to trend analysis.


