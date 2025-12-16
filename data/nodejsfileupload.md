# File Upload in Node.js

## Introduction

Handling file uploads is a common requirement in web applications. This tutorial covers uploading files using multer, handling different file types, validation, and best practices.

---

## Using Multer

### Installation

    npm install multer

### Basic Setup

    const express = require('express');
    const multer = require('multer');
    const app = express();
    
    // Configure storage
    const storage = multer.diskStorage({
      destination: (req, file, cb) => {
        cb(null, 'uploads/');
      },
      filename: (req, file, cb) => {
        const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
        cb(null, file.fieldname + '-' + uniqueSuffix);
      }
    });
    
    const upload = multer({ storage: storage });
    
    // Single file upload
    app.post('/upload', upload.single('avatar'), (req, res) => {
      res.json({
        success: true,
        file: req.file
      });
    });

---

## Upload Types

### Single File

    app.post('/upload', upload.single('file'), (req, res) => {
      if (!req.file) {
        return res.status(400).json({ error: 'No file uploaded' });
      }
      res.json({
        success: true,
        filename: req.file.filename,
        path: req.file.path
      });
    });

### Multiple Files

    app.post('/upload-multiple', upload.array('files', 5), (req, res) => {
      if (!req.files || req.files.length === 0) {
        return res.status(400).json({ error: 'No files uploaded' });
      }
      res.json({
        success: true,
        files: req.files.map(file => ({
          filename: file.filename,
          size: file.size
        }))
      });
    });

### Multiple Fields

    const uploadFields = upload.fields([
      { name: 'avatar', maxCount: 1 },
      { name: 'gallery', maxCount: 5 }
    ]);
    
    app.post('/upload-fields', uploadFields, (req, res) => {
      res.json({
        avatar: req.files.avatar,
        gallery: req.files.gallery
      });
    });

---

## File Validation

### File Type Validation

    const fileFilter = (req, file, cb) => {
      const allowedTypes = /jpeg|jpg|png|gif/;
      const extname = allowedTypes.test(
        path.extname(file.originalname).toLowerCase()
      );
      const mimetype = allowedTypes.test(file.mimetype);
      
      if (mimetype && extname) {
        return cb(null, true);
      } else {
        cb(new Error('Only image files are allowed'));
      }
    };
    
    const upload = multer({
      storage: storage,
      fileFilter: fileFilter,
      limits: {
        fileSize: 5 * 1024 * 1024 // 5MB
      }
    });

### File Size Limits

    const upload = multer({
      storage: storage,
      limits: {
        fileSize: 5 * 1024 * 1024, // 5MB
        files: 5 // Max 5 files
      }
    });

---

## Memory Storage

    const storage = multer.memoryStorage();
    const upload = multer({ storage: storage });
    
    app.post('/upload', upload.single('file'), (req, res) => {
      // File is in req.file.buffer
      const fileBuffer = req.file.buffer;
      
      // Process or save to cloud storage
      // Example: Upload to S3, Cloudinary, etc.
    });

---

## Complete Example

    const express = require('express');
    const multer = require('multer');
    const path = require('path');
    const fs = require('fs');
    
    const app = express();
    
    // Ensure upload directory exists
    const uploadDir = 'uploads';
    if (!fs.existsSync(uploadDir)) {
      fs.mkdirSync(uploadDir, { recursive: true });
    }
    
    // Configure storage
    const storage = multer.diskStorage({
      destination: (req, file, cb) => {
        cb(null, uploadDir);
      },
      filename: (req, file, cb) => {
        const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
        const ext = path.extname(file.originalname);
        cb(null, file.fieldname + '-' + uniqueSuffix + ext);
      }
    });
    
    // File filter
    const fileFilter = (req, file, cb) => {
      const allowedTypes = /jpeg|jpg|png|gif|pdf|doc|docx/;
      const extname = allowedTypes.test(
        path.extname(file.originalname).toLowerCase()
      );
      const mimetype = allowedTypes.test(file.mimetype);
      
      if (mimetype && extname) {
        cb(null, true);
      } else {
        cb(new Error('Invalid file type'));
      }
    };
    
    // Configure multer
    const upload = multer({
      storage: storage,
      limits: {
        fileSize: 10 * 1024 * 1024 // 10MB
      },
      fileFilter: fileFilter
    });
    
    // Upload endpoint
    app.post('/api/upload', upload.single('file'), (req, res) => {
      if (!req.file) {
        return res.status(400).json({
          success: false,
          error: 'No file uploaded'
        });
      }
      
      res.json({
        success: true,
        data: {
          filename: req.file.filename,
          originalName: req.file.originalname,
          size: req.file.size,
          path: req.file.path
        }
      });
    });
    
    // Error handling
    app.use((error, req, res, next) => {
      if (error instanceof multer.MulterError) {
        if (error.code === 'LIMIT_FILE_SIZE') {
          return res.status(400).json({
            success: false,
            error: 'File too large'
          });
        }
      }
      res.status(400).json({
        success: false,
        error: error.message
      });
    });

---

## Cloud Storage Integration

### Upload to Cloudinary

    const cloudinary = require('cloudinary').v2;
    const { CloudinaryStorage } = require('multer-storage-cloudinary');
    
    cloudinary.config({
      cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
      api_key: process.env.CLOUDINARY_API_KEY,
      api_secret: process.env.CLOUDINARY_API_SECRET
    });
    
    const storage = new CloudinaryStorage({
      cloudinary: cloudinary,
      params: {
        folder: 'uploads',
        allowed_formats: ['jpg', 'png', 'jpeg', 'gif']
      }
    });
    
    const upload = multer({ storage: storage });

---

## Best Practices

1. **Validate file types** and sizes
2. **Sanitize filenames** to prevent security issues
3. **Use cloud storage** for production
4. **Implement virus scanning** for uploaded files
5. **Set appropriate limits** on file size and count
6. **Generate unique filenames** to prevent overwrites
7. **Handle errors** gracefully
8. **Clean up** old/unused files
9. **Use CDN** for serving uploaded files
10. **Implement progress tracking** for large files

---

## Conclusion

File upload is essential for many applications. Use multer for handling uploads, validate files properly, and consider cloud storage for production applications.

