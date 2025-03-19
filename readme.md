This is a larger project that is capable of running a poster shop without any human intervention, from the inception of the design, to the listing in the online store, posting on Social media, and even creating and shipping the product to the customer. 

It's extensive so I'll try to summarize as best as possible. 

It has some cool functionalities including:

1. Perfoming QA on the generated artwork to avoid low-quality pieces being posted (this isn't perfect but can filter out the most egregious examples).
2. Generating different images based on the day of the week, with completely random prompts to ensure you never get the same image twice.
3. Generating Mockups, along with watermarked images.



The autoamtion flow is a six step process:
1. Design Generation.json
   
      This is the meat and potatoes of the process:
      - The automation generates five completely random prompts using the wildcard document (Art Themes.xlsx) and ChatGPT.
      - It provides these prompts to the Stable Diffusion API to generate the images.
      - It runs the images through Everypixel API to get a quality score and filter out the worst offenders.
      - The images are recorded and saved on sheets and drive.

2. Image Recording.json

    This is a simple scenario that checks the drive folder for any files and simply records them. This allows me to not only use the automation, but to add in additional art as well. As long as it's in the drive folder it will be included.

3. Image Processing.json

    This workflow resizes the file if it's too large to be posted in future steps. Additionally, it uses ChatGPT to give the piece a title, description, and social media caption.

4. Publish Listing.json

    This scenario is designed to publish our new art piece on Printify. To do this, I had to develop a creative solution to properly price each product while also including everything required on the listing without manual intervention. to do this the        automation takes a preset profit % and creates a dummy listing in order to grab the correct production costs. It then calculates the price given store fees, costs, and the desired profit. 

![chrome_X1R9J0sA0x](https://github.com/user-attachments/assets/b45f2472-d964-4342-9e1a-df050cdb73ee)

  The scenario then creates the real listing, publishes it to our Etsy store, and deactivates it immediately, in preperation for our next step.

![image](https://github.com/user-attachments/assets/0520a08f-181f-41a0-89bb-f97aedc521d9)


5. Listing Images.json

    This pipeline is where we get our mockups from for the store. It uses a mix of watermarked images from the Cloudconvert API, and actual mockups from the Dynamic Mockups API and Printifys default options. Once complete it uploads them to the listing       from the previous step and activates the listing.

![image](https://github.com/user-attachments/assets/ae96f44e-d5c5-4448-8335-c39b858f5288)
![image](https://github.com/user-attachments/assets/4e6de0e4-90d2-4a80-bb62-4c304e4d0128)
![chrome_k0gc6As6mQ](https://github.com/user-attachments/assets/9031c944-6347-42c4-bc8c-ca7756f4f919)
![chrome_qeef8JcGEN](https://github.com/user-attachments/assets/c0aa72da-8172-41d0-be04-71b46ce486c4)
![chrome_1YqwHhyl0B](https://github.com/user-attachments/assets/d53f2690-2cea-483d-aae0-051abdaee805)


6. Socials Posting.json
    The final step in the pipeline is to post the art on Social Media. It's set up to post on Instagram, Pinterest, and Deviant art. 

   ![image](https://github.com/user-attachments/assets/5f66d88c-ee99-46bb-a84a-765767b8109a)
