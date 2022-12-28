# Image Uploading

All api routes (At least as of Next.js 13) are located in the `pages/api` folder. Subsequent folders/files are automatically added as a route.

Example: `pages/api/animals/cat/siamsese.js === localhost:3000/api/animals/cat/siamese`

## Handler Function

The basic function that handles function goes as follows:

`export default async function handler(req, res) {}`

- `req` is the request object sent to the handler (usually the client)
  - You can access the request method with `req.method`
- `res` is the response object sent in response to the receive request object (usually sent to the client)

## Next Config

### Config Object

The config object that you export from a module is used to configure specific parts of the application, such as API endpoints or individual pages. It is a way to provide configuration options at a local level within the application. In the backend it is exported alongside your handler function. Example:

```
export const config = {};

export default async function handler(req, res) {...}

```

### Next.Config.JS

## Images

### Users Uploading Images

If you want to enable your users to upload images do the following:

1. `npm i nanoid`
2. Inside the handler:

```
export const config = {
  api: {
    bodyParser: {
      sizeLimit: '10mb', // Allows larger images to be uploaded in this route
    },
  },
};

export default async function handler(req, res) {
    try {
        // First check if there's an image
        let { image } = req.body;

        if (!image) {
        return res.status(500).json({ message: 'No image provided' });
        }

        // Check the image data as it should be encoded in Base64
        const contentType = image.match(/data:(.*);base64/)?.[1];
        const base64FileData = image.split('base64,')?.[1];

        if (!contentType || !base64FileData) {
        return res.status(500).json({ message: 'Image data not valid' });
        }

        /* Upload Image logic here (will vary if using cockroachDB, supabase, aws, etc.) */

    } catch (e) {
      res.status(500).json({ message: "Something went wrong" });
    }
}
```
