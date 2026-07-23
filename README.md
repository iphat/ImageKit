postRouter.js

    npm i multer @imagekit/nodejs

By default, an Express server cannot process files sent from the frontend using multipart/form-data. We use Multer middleware to parse the incoming file data, making it available in req.file or req.files

    const multer = require("multer");
    const upload = multer({storage:multer.memoryStorage()});

    postRouter.post("/posts",upload.single("iphat"), postController.createPostController);
    module.exports = postRouter

upload.single("image")         Upload one file.
upload.array("images", 5)      Upload up to 5 files.
upload.fields([...])           Upload multiple fields with different names.
upload.none()                   Accept only text fields, no files.  


postController.js

     const ImageKit = require("@imagekit/nodejs");
     const  {toFile} = require("@imagekit/nodejs");


     const imagekit = new ImageKit({
        privateKey: process.env.IMAGEKIT_PRIVATE_KEY
     })

    async function createPostController(req,res){
     const file = await imagekit.files.upload({
        file: await toFile(Buffer.from(req.file.buffer)),
        fileName: "Test",
      })
      res.send(file);
    }

    module.exports = {
        createPostController
    }
