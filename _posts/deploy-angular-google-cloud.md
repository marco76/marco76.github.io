
# How to deploy your Angular app in the Google Cloud Storage?

You created a beautiful Angular (or React or Html or ...) App and you need to deploy it on the web.

You decided to use a Cloud service because the app has to be scalable to millions of daily users.

## Create your bucket 

You can follow the official guide:
(https://cloud.google.com/storage/docs/hosting-static-website)

Remember to set the [index page](https://cloud.google.com/storage/docs/hosting-static-website#index-page), for an Angular website *you have to fill the [error page](https://cloud.google.com/storage/docs/hosting-static-website#error-page) with your index page* (the framework manages the urls).

## Build your Angular app
No surprise here:

```console
ng build -prod
```

## Prepare your bucket

When you copy your files to the Google storage your files are not accessible to anonymous users.

For your public website you should change the default authorization assigned to the files copied in the storage:

```console
gsutil defacl ch -u AllUsers:R gs://www.[domain-name.com]
```
This instruction doesn't modify the rights on the existing files in the bucket.

## Copy your files

To copy your angular application from the main directory of your app:

```console
gsutil -m cp ./dist/* gs://www.[domain-name.com]
```

## Update your registry

[As documented](https://cloud.google.com/storage/docs/hosting-static-website#cname) you should update the CNAME of your domain.

With this operation your website will work with the prefix www (e.g. www.domain-name.com) but it won't work without (e.g. domain-name.com).

To solve the issue you have to *create an URL redirect* (config vary for each provider) from @ to http://www.domain-name.com

## Java: from the war to the bucket
With the 'previous' monolithic approach it was meaningful to include your frontend inside a .war or .jar using some acrobatic maven operations.

With the cloud that is dominating the new enterprises' architectures, the frontend should not be deployed with the backend.





