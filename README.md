# Repro for Firebase-Tools issue #6636

## Versions

firebase-tools: v13.0.2<br>
node: v20.10.0

## Steps to reproduce

1. Run `firebase deploy --project <project_id>`.
1. Deployment goes through without any issues.
1. Change contents of `public/index.html`.
1. Run `firebase deploy --project <project_id>` again. Logs will show:

```
i  functions: Skipping the deploy of unchanged functions with experimental support for skipdeployingnoopfunctions
✔  functions[helloWorld(us-central1)] Skipped (No changes detected)
i  functions: cleaning up build files...
⚠  functions: Unhandled error cleaning up build images. This could result in a small monthly bill if not corrected. You can attempt to delete these images by redeploying or you can delete them manually at https://console.cloud.google.com/gcr/images/<project_id>/us/gcf
i  hosting[<project_id>]: finalizing version...

Error: HTTP Error: 400, Cloud Run service `helloWorld` does not exist in region `us-central1` in this project.
```

Notes:

When checking the Google Cloud console for Cloud Run services, a Cloud Run service with the name of `helloworld` is active.

- Changing the function name from `helloWorld`(camel case) to `helloworld`(lowercase only) works around the issue(?)

Running `firebase deploy --only hosting --project <project_id>` works fine.

Issue only happens when `skipdeployingnoopfunctions` occurs

- When making a change in `functions/index.js` to prevent
  `skipdeployingnoopfunctions`, no errors is raised when running `firebase deploy --project <project_id>`
