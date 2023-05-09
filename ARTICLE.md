# [UPDATE] How to setup Amazon S3 Upload Provider Plugin for your Strapi App

| Title                                | How to Setup Amazon S3 Upload Provider Plugin for your Strapi App                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Author                               | Mark Munyaka                                                                                                                                                                                                                                                                                                                                                                          |
| Image                                | ![](https://paper-attachments.dropbox.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659901656941_how-to-set-up-amazon-s3-upload-provider-plugin.jpg)<br><br>![](https://paper-attachments.dropbox.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659901724366_mark-munyaka-how-to-set-up-amazon-s3-upload-provider-for-strapi.png) |
| Meta description                     | In this tutorial you will set up the [Amazon S3 Upload Provider Plugin](https://market.strapi.io/providers/@strapi-provider-upload-aws-s3) for your Strapi App to work with an Amazon S3 storage bucket                                                                                                                                                                               |
| Number of words - **excluding code** | 2,426                                                                                                                                                                                                                                                                                                                                                                                 |

# Outline

- Introduction
    - Objective of Tutorial
    - What is Strapi?
    - What is Amazon S3?
- Prerequisites
    - Knowledge Prerequisites
    - Software Prerequisites
    - Hardware Prerequisites
    - Additional Prerequisites
- Step 1: Setup Strapi
- Step 2: Install and Configure S3 Upload Provider
    - Install AWS S3 Provider
    - Configure AWS S3 Provider
    - Security Middleware Configuration
- Step 3: Set up AWS
    - Create AWS Account
    - Create Administrator IAM Admin User and Group
- Step 4: Create an IAM User for your Strapi App
    - Set User Details
- Step 5: Set up Permissions for Amazon S3
    - Create Group
    - Add tags (optional)
    - Retrieve Credentials `Access key ID` and `Secret access key`
- Step 6: Create Amazon S3 Storage Bucket
- Step 7: Update Strapi S3 Configuration
    - Update Environment Variables
    - Update Middleware
- Step 8: Test your Uploads
    - Upload an image
    - Delete an image
- Optional
- Conclusion

In this tutorial, I will show you how to set up the [Amazon S3 Upload Provider Plugin](https://market.strapi.io/providers/@strapi-provider-upload-aws-s3) for your Strapi App. 

You will first set up and configure an S3 storage bucket for your app. Then, you'll create a Strapi App. Next, you will install and configure the Amazon S3 Upload Provider Plugin. Finally, you will test the plugin by uploading some images to your Strapi backend. 

By the end of this tutorial, you should be able to use the Amazon S3 Upload Provider Plugin with any Strapi project.

## What is Strapi?

[Strapi is the leading open-source headless Content Management System (CMS).](https://strapi.io/) It is 100% Javascript, based on [Node.js](https://nodejs.dev/), and used to build RESTful APIs and GraphQL. It also has a cloud SAAS, ☁ [Strapi Cloud](https://strapi.io/cloud) ☁ - a fully managed, composable, and collaborative platform to boost your content velocity!

Strapi enables developers to build projects faster by providing a customizable API out of the box and giving them the freedom to use their favorite tools. Content teams use Strapi to autonomously manage all types of content and distribute it from one CMS to any channel be it websites, mobile apps, or connected devices.

A powerful feature of [Strapi](https://strapi.io/features) is its flexibility and extensibility. This is provided through plugins hosted on the [Strapi Market](https://market.strapi.io/). Plugins help add more functionality to your Strapi app. One of such features is uploading your media assets to a cloud provider using the Strapi dashboard.
Sometimes, you may want to host your app's media files in a different location from where your Strapi app is hosted. This is useful to save on storage, memory, and compute resource usage. Also, you can leverage cloud-based solutions like Content Delivery Networks (CDN) and cloud storage buckets for faster and more reliable loading of your media assets.

## What is Amazon S3?

[Amazon AWS](https://aws.amazon.com/) is the biggest cloud provider. It offers [Amazon S3](https://aws.amazon.com/s3), a cloud storage service you can use to host your media assets. Amazon S3 is scalable, reliable, secure, and performant. The Strapi Team built an Amazon S3 Upload Provider Plugin to help with uploading your media assets to your S3 storage bucket.

## Prerequisites

To follow along with this tutorial you need the following:

### Knowledge Prerequisites:

- [Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Node.js](https://nodejs.dev/learn)
- [Shell (Bash)](https://www.gnu.org/software/bash/manual/bash.html)
- Strapi, follow the [Quick Start Guide](https://docs.strapi.io/dev-docs/quick-start) to familiarize yourself with Strapi.
- [Git](https://git-scm.com/book/en/v2)

### Software Prerequisites:

- Operating System (Any of the following are supported):
    - Ubuntu >= 18.04 (LTS-Only)
    - Debian >= 10.x
    - CentOS/RHEL >= 8
    - macOS Mojave or newer (ARM not supported)
    - Windows 10 or 11
    - Docker - [docker repo](https://github.com/strapi/strapi-docker)
    - *I used Ubuntu 20.04 LTS for this tutorial.*
- **Node v14.x.x or v16.x.x or v18.x.x** (LTS Only)
- Odd-number releases of Node are not supported (e.g. v13, v15). Download Node from the [Download | Node.js](https://nodejs.org/en/download/) page. *I used Node v18.16.0*
- **npm or yarn** *I used npm v8.5.0. npm ships with your Node installation.* If you prefer yarn, install it as an npm package. [Check Installation | Yarn](https://classic.yarnpkg.com/en/docs/install).
- **Strapi v4.x.x** (*I used Strapi v4.10.2. You will install Strapi using `npm` or `yarn`.*)
- A text editor of your choice
- Git for Version control [Download Git from the [Git - Downloads](https://git-scm.com/downloads) page]
- **Git Bash (for Windows users)**
- Git Bash ships with your Git download. The tutorial uses Unix shell commands. Git Bash can run UNIX shell commands in Windows.
- Web Browser (any Chromium-based browser)

### Hardware Prerequisites
- At least 1 CPU core (Highly recommended at least 2)
- At least 2GB of RAM (Moderately recommended 4)
- Minimum required storage space recommended by your OS or 32 GB of free space

### Additional Prerequisites

You will also need an AWS Account. A credit card is required to open one. AWS offers a 12-month free trial to test out the Amazon S3 bucket with 5GB of free storage.

## Step 1: Set up Strapi

We will start off by creating a Strapi app.

> **NOTE**: If you prefer testing the ready-made app, you can clone the [Github repo](https://github.com/Marktawa/strapi-aws-s3).

- Open up your terminal and navigate to your home folder.
```bash
cd $HOME
```

- Create a directory to store your project, name it `strapi-aws-s3`.
```bash
mkdir strapi-aws-s3
```

- Change the directory to the newly created directory, `strapi-aws-s3`.
```bash
cd strapi-aws-s3
```

- Create your Strapi app.
```bash
npx create-strapi-app backend --quickstart
```
    
*OR*

```bash   
yarn create strapi-app backend --quickstart
```

This command creates your Strapi app in the folder named `backend`. The `--quickstart` flag sets up your Strapi app with an [SQLite](https://www.sqlite.org/index.html) database.

Your folder structure in `strapi-aws-s3` after installing Strapi should look similar to this:

```bash
.
├── backend
│   ├── build
│   ├── .cache
│   ├── config
│   ├── data
│   ├── database
│   ├── .editorconfig
│   ├── .env
│   ├── .env.example
│   ├── .eslintignore
│   ├── .eslintrc
│   ├── favicon.ico
│   ├── .gitignore
│   ├── node_modules
│   ├── package.json
│   ├── public
│   ├── README.md
│   ├── src
│   ├── .strapi-updater.json
│   ├── .tmp
│   └── yarn.lock
├── .git
├── LICENSE
└── README.md
```

After installation, your app should start automatically. Visit [http://localhost:1337/admin](http://localhost:1337/admin) in your browser and register your details in the Strapi Admin Registration Form.

![Strapi Admin Registration](https://res.cloudinary.com/craigsims808/image/upload/v1683635835/articles/fox-oil/strapi-admin-reg_fh3p7o.png)


**NOTE:** If Strapi did not start automatically, run these commands:

```bash
cd backend
npm run develop
``` 
*OR*
```bash
cd backend
yarn develop
```

> `npm run develop` starts your Strapi project development server in **watch mode.** **Watch mode** changes in your Strapi project will trigger a server restart.

Creating a new user logs you into the Strapi Dashboard.

![Strapi Dashboard](https://res.cloudinary.com/craigsims808/image/upload/v1683635836/articles/fox-oil/strapi-dashboard_kmpzah.png)

The left-hand side of the Strapi dashboard lists the **Content Manager** containing your content types. The **PLUGINS** option lists the plugins installed in your project. **Content-Type Builder** Plugin and **Media Library** Plugin are installed by default in your Strapi app. 

![Media Library Upload Plugin](https://res.cloudinary.com/craigsims808/image/upload/v1683635836/articles/fox-oil/media-library-modal_vqsebe.png)

The **Media Library** Plugin is what you will use to upload images to your Amazon S3 bucket. If the Amazon S3 Upload Provider Plugin is working, images you upload in the **Media Library** should appear in your S3 bucket automatically.

## Step 2 - Install and Configure '@strapi/provider-upload-aws-s3' Plugin

### Install AWS S3 Provider

Stop your strapi server by pressing `Ctrl+C` in your terminal. Install `@strapi/provider-upload-aws-s3`:

```bash
cd backend
npm install @strapi/provider-upload-aws-s3 --save
```

*OR*

```bash
cd backend
yarn add @strapi/provider-upload-aws-s3
```

### Configure AWS S3 Provider

Create a file named `plugins.js` in your `config` folder

```bash
touch config/plugins.js
```

Add this configuration code to `plugins.js`:

```js
// ~/strapi-aws-s3/backend/config/plugins.js
    
module.exports = ({ env }) => ({
      upload: {
        config: {
          provider: 'aws-s3',
          providerOptions: {
            accessKeyId: env('AWS_ACCESS_KEY_ID'),
            secretAccessKey: env('AWS_ACCESS_SECRET'),
            region: env('AWS_REGION'),
            params: {
              ACL: env('AWS_ACL', 'public-read'),
              signedUrlExpires: env('AWS_SIGNED_URL_EXPIRES', 15 * 60),
              Bucket: env('AWS_BUCKET'),
            },
          },
          actionOptions: {
            upload: {},
            uploadStream: {},
            delete: {},
          },
        },
      },
});
```

This enables the plugin to work in your app. 

`AWS_ACCESS_KEY_ID`, `AWS_ACCESS_SECRET`, `AWS_REGION`, and `AWS_BUCKET` are environment variables. You will later assign their values in the `.env` file for your Strapi project after setting up the Amazon S3 bucket.

### Security Middleware Configuration

Due to the default settings in the Strapi Security Middleware, you will need to modify the `contentSecurityPolicy` settings to properly see thumbnail previews in the Media Library. You should replace `strapi::security` string with the object below instead as explained in the [middleware configuration](https://docs.strapi.io/dev-docs/configurations/middlewares#loading-order) documentation.

Edit the `middlewares.js` file in your `config` folder:

```js
// ~/strapi-aws-s3/backend/config/middlewares.js
    
module.exports = [
  'strapi::errors',
  /* Replace 'strapi::security', with this snippet */
  /* Beginning of snippet */
  {
    name: 'strapi::security',
    config: {
      contentSecurityPolicy: {
        useDefaults: true,
        directives: {
          'connect-src': ["'self'", 'https:'],
          'img-src': [
            "'self'",
            'data:',
            'blob:',
            'dl.airtable.com',
            'yourBucketName.s3.yourRegion.amazonaws.com',
          ],
          'media-src': [
            "'self'",
            'data:',
            'blob:',
            'dl.airtable.com',
            'yourBucketName.s3.yourRegion.amazonaws.com',
          ],
          upgradeInsecureRequests: null,
        },
      },
    },
  },
  /* End of snippet */
  'strapi::cors',
  'strapi::poweredBy',
  'strapi::logger',
  'strapi::query',
  'strapi::body',
  'strapi::session',
  'strapi::favicon',
  'strapi::public',
];
```

Later you will replace `'yourBucketName.s3.yourRegion.amazonaws.com'` with the link to your S3 bucket. Refer to [AWS S3 | Strapi Market](https://market.strapi.io/providers/@strapi-provider-upload-aws-s3) for more information on configuring the plugin.

## Step 4: Set up AWS

The next step here is to set up AWS as part of our project.

### Create AWS Account

Sign up for an AWS account on the [AWS Console - Signup](https://portal.aws.amazon.com/billing/signup) page if you don’t have one already. Refer to the [Create and activate an AWS account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) guide for additional information related to the AWS account creation process.

### Create Administrator IAM Admin User and Group**

Best practices for using **AWS Amazon** services state to not use your root account user and to use instead the [IAM (AWS Identity and Access Management) service](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html). Your root user is therefore only used for a very few [select tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html). For example, for **billing**, you create an **Administrator user and Group** for such things. Other routine tasks can also be done with a **regular IAM User**.

Follow the “[Creating your first IAM admin user and user group - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)” guide to create an IAM admin user and user group using your AWS root account in the AWS Management Console.

## Step 5: Create an IAM User for your Strapi App

### Set User Details:

- Sign out of your **root user** account and sign in to your **Administrator** user account you just created.
![Sign in as IAM Administrator user](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659989175854_image.png)

- Visit the IAM Console by selecting **Services** then **All services,** scroll down and select **IAM** or go here to [**IAM Console**](https://console.aws.amazon.com/iam/home).
![Go to IAM Console](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659989698555_image.png)

- Click on **Users** in the left-hand menu and select **Add User.**
![Select Add users in Users IAM Console](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659991053400_image.png)

- Provide a **Username**. I used `Strapi-Admin`.
- **Access Type**: Check both `Programmatic access` and `AWS Management Console access`.
- `Autogenerate a password` or click `Custom password` and provide one.
- **OPTIONAL:** For simplicity, `uncheck` the **Require password reset**.
- Click `Next: Permissions`.
![Set user details for Strapi Admin](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659991228325_image.png)


## Step 6: Set up Permissions for Amazon S3

### Create Group

Follow the steps below to set up permissions for Amazon S3.

- Click `Create group` and name it. Then, choose appropriate policies under **Policy Name**:
    - Search for `s3` and check `AmazonS3FullAccess`.
    - Click `Create group`.
- Click to `Add user to group` and check the `Developers` group to add the new user.
- Click `Next: Tags`.
![Enable Access to Amazon S3](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659991368722_image.png)

- Click `Add user to group` and check the `Developers` group to add the new user.
![Check newly added user group](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659991688705_image.png)

### Add Tags

**This step is** optional **and based on your workflow and project scope.** To begin, click `Next: Review`.

### Retrieve Credentials 'Access Key ID' and 'Secret Access Key'**

Review the information and ensure it is correct. Use `Previous` to correct anything. If satisfied, click `Create user`.

![Review User Information](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659991936705_image.png)

If you do not do these steps, you will have to reset your `Access key ID` and `Secret access key` later.

Download the `.csv` file and store it in a safe place. This contains the username, login link, access key ID and secret access key.

You can decide to keep these details in your password manager.

![Retrieve User Credentials](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659992374753_image.png)

- Click on the `AWS Management Console Access sign-in link`. This will log you out of `Administrator`.

For more information, check out [creating a regular user for the creation and management of your Strapi project](https://docs.strapi.io/developer-docs/latest/setup-deployment-guides/deployment/hosting-guides/amazon-aws.html#_2-next-create-a-regular-user-for-the-creation-and-management-of-your-strapi-project).

## Step 7: Create Amazon S3 Storage Bucket

Log into `your AWS Management Console` as the new user you just created `Strapi-Admin`.

![Sign in as Strapi-Admin IAM user](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659992702356_image.png)


Go to **Services**, click **All services,** scroll down, and select **S3 Scalable Storage in the Cloud** to open up the Amazon S3 Console.

![Select S3 in Services Menu](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659993216424_image.png)


Click on **Create bucket** in the Amazon S3 console.

![Create bucket](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659993404248_image.png)


Give your bucket a unique name. I named mine `strapi-aws-s3-images-bucket`. Select the most appropriate region for your bucket.

![Name your bucket](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659993588468_image.png)


For **Object Ownership**, select **ACLs enabled** to enable access to the S3 bucket from your Strapi backend. Select **Bucket owner preferred** for the plugin to work.

![S3 Bucket Object Ownership](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659999041730_image.png)


Under **Block Public Access settings for this bucket**, do the following:

- Uncheck **Block all public access** and set the permissions as follows:
    - Uncheck **Block public access to buckets and objects granted through new access control lists (ACLs)** (Recommended)
    - Uncheck **Block public access to buckets and objects granted through any access control lists (ACLs)**
    - Check **Block public access to buckets and objects granted through new public bucket policies**
    - Check **Block public and cross-account access to buckets and objects through any public bucket policies**
- Check *I acknowledge that the current settings might result in this bucket and the objects within becoming public* under the **Turning off block all public access might result in this bucket and the objects within becoming public** warning.
![Block Public Access settings for this bucket](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659994379399_image.png)


Leave the rest of the settings as is and click **Create bucket.** Your new bucket should now be listed in the Amazon S3 Console.

![Newly created bucket listed in the console](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1659994573996_image.png)

## Step 9: Update Strapi S3 Configuration

### Update Environment variables

Update your Strapi backend with new credentials from your newly created S3 bucket. Update `.env` with the following:

```yml
# ~/strapi-aws-s3/backend/.env */
# Add this after the last line
    
AWS_ACCESS_KEY_ID=<Access Key ID>
AWS_ACCESS_SECRET=<Secret access key>
AWS_REGION=eu-west-2
AWS_BUCKET=strapi-aws-s3-images-bucket
```

> **NOTE:** Replace the AWS credentials and with the credentials in the **.csv** file you downloaded after creating an IAM role for your S3 bucket. For AWS_REGION input the region where your bucket is hosted. My bucket is hosted at eu-west-2.

### Update Middleware

Update the `middlewares.js` file in your `config` folder by replacing `yourBucketName.s3.yourRegion.amazonaws.com` with the link to your S3 bucket. In my case the link becomes `strapi-aws-s3-images-bucket.s3.eu-west-2.amazonaws.com` like so:

```js
// ~/strapi-aws-s3/backend/config/middlewares.js

module.exports = [
  'strapi::errors',
  {
    name: 'strapi::security',
    config: {
      contentSecurityPolicy: {
        useDefaults: true,
        directives: {
          'connect-src': ["'self'", 'https:'],
          'img-src': [
            "'self'",
            'data:',
            'blob:',
            'dl.airtable.com',
            'strapi-aws-s3-images-bucket.s3.eu-west-2.amazonaws.com', // change here
          ],
          'media-src': [
            "'self'",
            'data:',
            'blob:',
            'dl.airtable.com',
            'strapi-aws-s3-images-bucket.s3.eu-west-2.amazonaws.com', // change here
          ],
          upgradeInsecureRequests: null,
        },
      },
    },
  },
  'strapi::cors',
  'strapi::poweredBy',
  'strapi::logger',
  'strapi::query',
  'strapi::body',
  'strapi::session',
  'strapi::favicon',
  'strapi::public',
];
```

Build your backend and start your server.

```bash
npm run build
npm run develop
```

#OR

```
yarn build
yarn develop
```

## Step 10: Test your Uploads

### Upload An Image

Visit the **Media Library** plugin page, [http://localhost:1337/admin/plugins/upload](http://localhost:1337/admin/plugins/upload). Click **+ Add new assets.** Browse for an image you want to add from your computer and select **Upload 1 asset to the library**.

![Upload image to Amazon S3 bucket using Strapi Admin](https://res.cloudinary.com/craigsims808/image/upload/v1683635836/articles/fox-oil/upload-image_tzuj2r.png)


In a few moments, you should see your newly uploaded image in your **Media Library** as well as your Amazon S3 Bucket.

![Newly Uploaded Image in Media Library](https://res.cloudinary.com/craigsims808/image/upload/v1683635838/articles/fox-oil/uploaded-image-in-library_q3qoeh.png)


Visit [https://s3.console.aws.amazon.com/s3/buckets/*](https://s3.console.aws.amazon.com/s3/buckets/*)*?region=** to see the newly uploaded image in your S3 bucket.

![Newly Uploaded Image in Amazon S3 Bucket](https://res.cloudinary.com/craigsims808/image/upload/v1683635837/articles/fox-oil/uploaded-image-in-bucket-v1_smr6yq.png)


### Delete an Image

Delete the image from the Media Library. The image is automatically deleted from the Amazon S3 bucket.

![Delete image in Strapi Admin Dashboard](https://res.cloudinary.com/craigsims808/image/upload/v1683635836/articles/fox-oil/delete-image_zh11ca.png)


Confirm the deletion by refreshing the link to your bucket. The bucket is now empty. Deleting the image from the Strapi Admin Dashboard worked.

![No objects in Amazon S3 Bucket](https://paper-attachments.dropboxusercontent.com/s_DEA65C6CE85B8B99CB1CC70F901A2EB93FDCEBD4ABE886AF7F3FCEA073868233_1660001169657_image.png)


You have successfully set up the Amazon S3 Upload Provider Plugin.

## Optional

You can edit your `middleware.js` so that it uses environment variables for the `img-src` and `media-src` attributes like so:

```js
// ~/strapi-aws-s3/backend/config/middlewares.js

module.exports = [
  'strapi::errors',
  {
    name: 'strapi::security',
    config: {
      contentSecurityPolicy: {
        useDefaults: true,
        directives: {
          'connect-src': ["'self'", 'https:'],
          'img-src': [
            "'self'",
            'data:',
            'blob:',
            'dl.airtable.com',
            `${process.env.AWS_BUCKET}.s3.${process.env.AWS_REGION}.amazonaws.com`,
          ],
          'media-src': [
            "'self'",
            'data:',
            'blob:',
            'dl.airtable.com',
            `${process.env.AWS_BUCKET}.s3.${process.env.AWS_REGION}.amazonaws.com`, 
          ],
          upgradeInsecureRequests: null,
        },
      },
    },
  },
  'strapi::cors',
  'strapi::poweredBy',
  'strapi::logger',
  'strapi::query',
  'strapi::body',
  'strapi::session',
  'strapi::favicon',
  'strapi::public',
];
```

Update the CORS configuration for your S3 bucket so that the thumbnails for your media uploads can be displayed in your Strapi Admin Dashboard.

Go to your to your Strapi App bucket in your Amazon S3 console and select the Permissions tab.

![Select Permissions Tab](https://res.cloudinary.com/craigsims808/image/upload/v1683635836/articles/fox-oil/select-permissions_vmumy6.png)

Scroll down to the **Cross-origin resource sharing (CORS)** block and select **Edit**

![Edit CORS config](https://res.cloudinary.com/craigsims808/image/upload/v1683636391/articles/fox-oil/edit-cors_blnzkx.png)

Add the following JSON for CORS configuration.
```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": [],
    "MaxAgeSeconds": 3000
  }
]
```

This rule will allow GET requests from any origin which includes your Strapi backend. For more information check out [Strapi S3 Bucket CORS Configuration](https://market.strapi.io/providers/@strapi-provider-upload-aws-s3) and [Amazon S3 CORS Configuration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ManageCorsUsing.html).

Click on **Save changes** and your updated CORS configuration should be like this:

![Updated CORS](https://res.cloudinary.com/craigsims808/image/upload/v1683635834/articles/fox-oil/updated-cors_tvkosl.png)

>**NOTE:**
>
>In production, update the "AllowedOrigins" to your STRAPI URL

## Conclusion

In this tutorial, we created a Strapi project then installed and configured the **AWS S3 provider for Strapi uploads** (`@strapi/provider-upload-aws-s3`) plugin in our Strapi project folder. We then created an Amazon S3 bucket with the recommended IAM policies to manage it.

Finally, we tested our app and saw the ease with which we can upload and delete images in the Amazon S3 bucket using the Strapi Admin Media Library. Check out [the Github project repo](https://github.com/syboxdigital/strapi-aws-s3).

I hope this tutorial has provided enough information and insight to help you set up the Amazon S3 provider with full confidence. If you have any issues, feel free to comment. The Strapi community is always available for any issues.


