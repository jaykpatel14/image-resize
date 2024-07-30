
const { app } = require('@azure/functions');
const axios = require("axios");
const sharp = require('sharp');

app.http('jp_img_sz', {
    methods: ['GET', 'POST'],
    authLevel: 'anonymous',
    handler: async (request, context) =>
    {
        const imageUrl = request.query.get('img');
        context.log('Image: ${imageUrl}');

        if (imageUrl)
        {
            try {
                const array = await axios({url: imageUrl, responseType: 'arraybuffer'});
                const imageBuffer = Buffer.from(array.data, 'binary');
                const resizedImageBuffer = await sharp(imageBuffer).resize(250, 250).toBuffer();
                context.res = 
                {
                    status: 200,
                    headers: {
                        'Content-Type': 'image/png',
                        'Content-Disposition': 'inline; filename="resized.png"'
                    },
                    body: resizedImageBuffer,
                    isRow: true
                };
            }
            catch (error)
            {
                context.log('Error processing image: ${error.message}');
                context.res = 
                {
                    status: 500,
                    body: 'Error processing Image: ${error.message}'
                };
            }
        }
        else
        {
            context.res = 
            {
                status: 400,
                body: "Please pass an image_url, on the query string or in the request body"
            };
        }
        return context.res;
    }
});
