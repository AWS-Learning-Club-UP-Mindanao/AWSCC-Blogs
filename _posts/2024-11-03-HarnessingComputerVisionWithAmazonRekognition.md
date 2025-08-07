---
title: Harnessing Computer Vision with Amazon Rekognition
description: In this article, we will delve into the world of Computer Vision and learn about Amazon Rekognition and its facets. We will explore how to utilize Amazon Rekognition API to analyze images and use these analyses for our own consumption. With Amazon Rekognition, you can do a lot of things, such as face analysis, text extraction, video analysis, and more.
date: 2024-11-03
author: mark
categories: [Machine Learning]
tags: [Amazon Rekognition, Computer Vision]
pin: false
image: 
  path: /assets/img/computer-vision-with-amazon-rekognition/rekognition-1.jpg
---

# Harnessing Computer Vision with Amazon Rekognition

### **✨ Introduction**

Have you ever wondered how self-driving cars navigate busy streets or how your smartphone recognizes your face to unlock? These are real-life examples of computer vision in action. Computer vision is a field of computer science that focuses on enabling computers to identify and understand objects and people in images and videos. It seeks to perform and automate tasks mimicking how humans would see and interpret what they see.

Computer vision applications work by using algorithms trained on large amounts of visual data, such as images and videos. These algorithms are powered by machine learning, artificial intelligence, and deep learning. They recognize patterns in this visual data and use those patterns to determine the content of other images or videos. Computer vision can be run in the cloud or on-premises.

### **🔎 What is Amazon Rekognition?**

Amazon Rekognition is a cloud-based image and video analysis service that allows you to seamlessly integrate advanced computer vision capabilities into your own applications. The service is fully managed by AWS, is built on proven deep learning technology, and requires no machine learning expertise to use.
> Why should we use Amazon Rekognition when dealing with computer vision applications?

### **How Amazon Rekognition Leverages Computer Vision?**

When developing image or video analysis models, time becomes an essential factor. This process revolves around extensive data gathering, provisioning, and training of the models. These processes can potentially be time-consuming, ineffective, and costly. Amazon Rekognition addresses these challenges by offering pre-trained, high-performance computer vision models that are ready to use out of the box. You don’t have to worry, as Amazon Rekognition eliminates the need for extensive data collection and the lengthy training period associated with developing models from scratch.

By leveraging Rekognition, you can swiftly integrate powerful image and video analysis capabilities into your applications. It also saves time and reduces costs, and you can now focus on building solutions that deliver immediate value.

### **Major Key Features**

* **Face Detection and Analysis —**Detect, analyze, and compare faces, along with facial attributes, such as gender, age, and emotions.

* **Object and Scene Detection —**Detects and classifies objects, scenes, and concepts in images and videos.

* **Unsafe Content Detection** — Detect explicit, inappropriate, and violent content in images and videos.

* **Text Detection —**Detect and recognize printed and handwritten text in images in a variety of languages.

* **Face liveness** — Detect if a live user is present during face verification.

* **Custom Labels —**Build custom classifiers to detect objects specific to your use case, such as logos, products, and characters.

### Use cases

* **Face-Based User Identity Verification** — Verify user identities through image face comparison with reference face images. Beneficial for applications requiring identity verification.

* **Searchable Media Libraries** — Rekognition detects labels, objects, concepts, and scenes in images and videos. These labels can be made searchable using this visual content analysis. Useful for building searchable image and video libraries.

* **Face Liveness Detection —**Helps to ensure that a user is physically present in front of the camera and not a bad actor spoofing the user’s face. It can help detect spoof attacks that do not use a camera, such as printed photos, digital photos/videos, or pre-recorded or deepfake videos injected directly into the video capture subsystem.

* **Unsafe Content Detection** — Detect and filter explicit, inappropriate, and violent content. Uses labels for granular filtering based on business needs.

* **Text Detection** — Detect and extract text from images for use in visual searches or metadata extraction. This works with various fonts and styles. Detects orientation for handling text on signs and banners.

* **Custom Labels** — Identify custom objects, concepts, and scenes specific to business use cases, such as logo detection. Custom classifiers can be trained to handle niche or proprietary objects, resulting in higher accuracy on key objects than general classifiers.

### **Benefits**

* **Integrating powerful image and video analysis into your application** — Add accurate image and video analysis to apps that don’t require expertise. The Amazon Rekognition API allows for deep learning analysis without requiring machine learning knowledge. You can easily incorporate computer vision into web, mobile, and device applications.

* **Highly scalable image analysis** — Analyze millions of images to organize large visual data sets. Scalable to accommodate growing image libraries and traffic. You don’t have to plan for capacity, and *you only pay for what you use.*

* **Analyze and filter images based on properties** — Analyze and filter images based on quality, color, and visual content and detect image sharpness, brightness, and contrast.

* **Integration with other AWS services** — Amazon Rekognition integrates seamlessly with S3 and Lambda. You can use Amazon Rekognition APIs from Lambda to process images in Amazon S3 without moving data. Rekognition includes built-in scalability and security via AWS IAM.

### **💸 Pricing**

Amazon Rekognition is included in the Free Tier and offers 1000 images per month, as well as stores 1,000 face vector objects and 1,000 user vector objects per month. You can save more as usage scales via tiered pricing. For more detailed information regarding the pricing, refer [here](https://aws.amazon.com/rekognition/pricing/):

### **🔨 Short Demo**

To understand the capabilities of Amazon Rekognition, we will do a simple demonstration using the Amazon Rekognition API to do image analysis. By the end of this demonstration, you’ll be equipped with the knowledge to seamlessly integrate Amazon Rekognition into your own applications.
> This demonstration is cost-free if you are eligible and under the AWS Free Tier. If not, you may incur cost.

### **Prerequisites:**

To follow the tutorial smoothly, you must:

* have Linux (WSL, if you’re on Windows).

* have Jupyter Notebook and its kernel.

* have an AWS Account ( Under Free Tier, if possible). If not, you may refer to it [here](https://signin.aws.amazon.com/signup?request_type=register).

* have set AWS CLI in your machine. If not, you may refer to it [here](https://docs.aws.amazon.com/rekognition/latest/dg/setup-awscli-sdk.html).

### Let’s Roll Up Our Sleeves

Before we can start, we need to pick a language for our AWS SDK (Software Development Kit). In my case, we will choose Python, and we need to install its corresponding library related to SDK.

```terminal
pip install boto3
```

Then, we will create our playground directory to keep things organized.

```terminal
mkdir aws-rekognition-playground && cd aws-rekognition-playground
```

### Storing our Images

Next, let’s traverse to our AWS Management Console. We will create an S3 Bucket to place our images. You can also use your store images in your local system, but that will be a different topic. For now, we will only store one image in the bucket. For our reference image, you can download it [here](https://www.planetnatural.com/wp-content/uploads/2023/09/Asian-woman-with-cherry-blossom-trees.jpg), or you can use your own image as well. I also rename the image to sample_image for convenience, or you can rename it with anything.
> Amazon Rekognition Image operations can analyze images in .jpg or .png format.

Search S3in your AWS Management Console and click it.

![](https://cdn-images-1.medium.com/max/3044/1*UTFcCc-YVTxpAtmB331tcg.png)

Click Create Bucket and name the bucket rekognition-playground-bucket. Leave everything else by default.

![](https://cdn-images-1.medium.com/max/3832/1*5bsMUe9u6hA-tolr1PRceg.png)

![](https://cdn-images-1.medium.com/max/2360/1*d_dMzEtZXwLzOpzaQyGWMA.png)

Next, let’s upload our image now. Click Upload then click Add files , then click Upload at the very bottom.

![](https://cdn-images-1.medium.com/max/3836/1*EizvQ-gyFQacK-bAWK52YQ.png)

![](https://cdn-images-1.medium.com/max/2086/1*28jtbLidc_mpVeQxbQKCuw.png)
> Note: You pass image bytes to an Amazon Rekognition Image operation as part of the call or you reference an existing Amazon S3 object. If you use HTTP and pass the image bytes as part of an Amazon Rekognition Image operation, the image bytes should be a base64-encoded string. If you use the AWS SDK and pass image bytes as part of the API call, the need to base64-encode the image bytes depends on the language you use. More information [here](https://docs.aws.amazon.com/rekognition/latest/dg/images-information.html).

Congratulations! You now have aS3bucket with a file in it. Let’s now go back into the project directory we made a while ago.

### Going Back To Our Playground

Start by creating a Jupyter Notebook named rekognition-s3.ipynband open it in your favorite code editor. (I am using VS Code)

```terminal
touch rekognition-s3.ipynb && code .
```

Copy this code into your Jupyter Notebook and change the following lines in the code:

* Replace profile-namewith the name of your developer profile (in my case, default ).

* Replace photo-name with sample_image.jpgor the name of the image that you stored in the S3 bucket

* Replace bucket-name with rekognition-playground-bucket or the name of your S3 bucket.

  # aws-rekognition-playground/rekognition-s3.ipynb

```python
    import boto3

    def detect_labels(photo, bucket):

         session = boto3.Session(profile_name='profile-name')
         client = session.client('rekognition')
    
         response = client.detect_labels(Image={'S3Object':{'Bucket':bucket,'Name':photo}},
         MaxLabels=10,
         )
    
         return response['Labels']

    def main():
        photo = 'photo-name'
        bucket = 'bucket-name'
        label_count = detect_labels(photo, bucket)
        print("Labels detected: " + str(label_count))

    if **name** == "**main**":
        main()
```

Let’s break down and analyze the code above:

* import boto3 — Import the AWS SDK for Python

* session = boto3.Session(profile_name='profile-name' — Create a new session with the specified profile name

* client = session.client(‘rekognition’) — Create a Rekognition client

* client.detect_labels() — Use the DetectLabels operation built-in on Amazon Rekognition to detect labels in the image

* Image={'S3Object': {'Bucket': bucket, 'Name': photo}} — Specify the S3 bucket and image name

* MaxLabels=10 — Limit the number of labels to return

* return response['Labels'] — Return the Labelspart of the response

> DetectLabels operation has more optional parameters to customize the response, such as MinConfidence. More information in [here](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_DetectLabels.html).

Run the code in your Jupyter Notebook. You will probably see something like this or a one-liner JSON. When calling Amazon Rekognition API, it will return a response in JSON format.

```json
    [
       {
          "Name":"Dress",
          "Confidence":100.0,
          "Instances":[
             
          ],
          "Parents":[
             {
                "Name":"Clothing"
             }
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Apparel and Accessories"
             }
          ]
       },
       {
          "Name":"Fashion",
          "Confidence":100.0,
          "Instances":[
             
          ],
          "Parents":[
             
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Apparel and Accessories"
             }
          ]
       },
       {
          "Name":"Formal Wear",
          "Confidence":100.0,
          "Instances":[
             
          ],
          "Parents":[
             
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Apparel and Accessories"
             }
          ]
       },
       {
          "Name":"Gown",
          "Confidence":100.0,
          "Instances":[
             
          ],
          "Parents":[
             {
                "Name":"Clothing"
             },
             {
                "Name":"Dress"
             },
             {
                "Name":"Fashion"
             },
             {
                "Name":"Formal Wear"
             }
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Apparel and Accessories"
             }
          ]
       },
       {
          "Name":"Robe",
          "Confidence":99.97161865234375,
          "Instances":[
             
          ],
          "Parents":[
             {
                "Name":"Clothing"
             },
             {
                "Name":"Fashion"
             }
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Apparel and Accessories"
             }
          ]
       },
       {
          "Name":"Adult",
          "Confidence":99.62248992919922,
          "Instances":[
             {
                "BoundingBox":{
                   "Width":0.22904235124588013,
                   "Height":0.6312879323959351,
                   "Left":0.5492144823074341,
                   "Top":0.36671799421310425
                },
                "Confidence":99.62248992919922
             }
          ],
          "Parents":[
             {
                "Name":"Person"
             }
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Person Description"
             }
          ]
       },
       {
          "Name":"Female",
          "Confidence":99.62248992919922,
          "Instances":[
             {
                "BoundingBox":{
                   "Width":0.22904235124588013,
                   "Height":0.6312879323959351,
                   "Left":0.5492144823074341,
                   "Top":0.36671799421310425
                },
                "Confidence":99.62248992919922
             }
          ],
          "Parents":[
             {
                "Name":"Person"
             }
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Person Description"
             }
          ]
       },
       {
          "Name":"Person",
          "Confidence":99.62248992919922,
          "Instances":[
             {
                "BoundingBox":{
                   "Width":0.22904235124588013,
                   "Height":0.6312879323959351,
                   "Left":0.5492144823074341,
                   "Top":0.36671799421310425
                },
                "Confidence":99.62248992919922
             }
          ],
          "Parents":[
             
          ],
          "Aliases":[
             {
                "Name":"Human"
             }
          ],
          "Categories":[
             {
                "Name":"Person Description"
             }
          ]
       },
       {
          "Name":"Woman",
          "Confidence":99.62248992919922,
          "Instances":[
             {
                "BoundingBox":{
                   "Width":0.22904235124588013,
                   "Height":0.6312879323959351,
                   "Left":0.5492144823074341,
                   "Top":0.36671799421310425
                },
                "Confidence":99.62248992919922
             }
          ],
          "Parents":[
             {
                "Name":"Adult"
             },
             {
                "Name":"Female"
             },
             {
                "Name":"Person"
             }
          ],
          "Aliases":[
             
          ],
          "Categories":[
             {
                "Name":"Person Description"
             }
          ]
       },
       {
          "Name":"Flower",
          "Confidence":96.31294250488281,
          "Instances":[
             
          ],
          "Parents":[
             {
                "Name":"Plant"
             }
          ],
          "Aliases":[
             {
                "Name":"Blossom"
             }
          ],
          "Categories":[
             {
                "Name":"Plants and Flowers"
             }
          ]
       }
    ]
```

You can do whatever you want with the response, and you can customize it as needed to best fit your application. For starters, you can draw a bounding box over the detected labels based on the coordinates given in the response. But for now, let’s add print statements to organize our response into something much more readable.

Copy the code below in your Jupyter Notebook. Replace the necessary details, such as profile-name , photo-name and bucket-name.

```python
    # aws-rekognition-playground/rekognition-s3.ipynb
    
    import boto3
    
    def detect_labels(photo, bucket):
    
         session = boto3.Session(profile_name='profile-name')
         client = session.client('rekognition')
    
         response = client.detect_labels(Image={'S3Object':{'Bucket':bucket,'Name':photo}},
         MaxLabels=10,
         )
    
         print('Detected labels for ' + photo)
         print()
         for label in response['Labels']:
             print("Label: " + label['Name'])
             print("Confidence: " + str(label['Confidence']))
             print("Instances:")
    
             for instance in label['Instances']:
                 print(" Bounding box")
                 print(" Top: " + str(instance['BoundingBox']['Top']))
                 print(" Left: " + str(instance['BoundingBox']['Left']))
                 print(" Width: " + str(instance['BoundingBox']['Width']))
                 print(" Height: " + str(instance['BoundingBox']['Height']))
                 print(" Confidence: " + str(instance['Confidence']))
                 print()
    
             print("Parents:")
             for parent in label['Parents']:
                print(" " + parent['Name'])
    
             print("Aliases:")
             for alias in label['Aliases']:
                 print(" " + alias['Name'])
    
                 print("Categories:")
             for category in label['Categories']:
                 print(" " + category['Name'])
                 print("----------")
                 print()
    
         if "ImageProperties" in str(response):
             print("Background:")
             print(response["ImageProperties"]["Background"])
             print()
             print("Foreground:")
             print(response["ImageProperties"]["Foreground"])
             print()
             print("Quality:")
             print(response["ImageProperties"]["Quality"])
             print()
    
         return len(response['Labels'])
    
    def main():
        photo = 'photo-name'
        bucket = 'bucket-name'
        label_count = detect_labels(photo, bucket)
        print("Labels detected: " + str(label_count))
    
    if __name__ == "__main__":
        main()
```

Run the code and enjoy the detailed analysis of Amazon Rekognition on the image you have selected.

### **🧹 Cleanup**

After finishing the demonstration and experimenting, you can empty and delete the S3 bucket you have created.

### **🤖 Wrap up**

In this article, we delved into the world of Computer Vision and learned about Amazon Rekognition and its facets. We have explored how to utilize Amazon Rekognition API to analyze images and use these analyses for our own consumption. We have also utilized AWS SDK (Software Development Kit) with Amazon Rekognition and learned how versatile Amazon Rekognition is as it supports a wide variety of programming languages. With Amazon Rekognition, you can do a lot of things, such as face analysis, text extraction, video analysis, and more. For the official documentation of Amazon Rekognition, please refer to this [link](https://docs.aws.amazon.com/rekognition/).

I hope you had fun while reading this article and learned valuable concepts. Congratulations! You are now one step closer to the world of Computer Vision and AWS. Good luck on your journey!
> Don’t forget to “clap” this article if you enjoyed it! You can clap multiple times!
