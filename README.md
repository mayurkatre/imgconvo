Documentation for Image Conversion and Compression Tool:

Overview:-

This web application allows users to convert and compress images directly on their device. The features include:


-Choosing the output file format (JPEG, PNG, WebP).


-Setting image quality.


-Resizing images using percentages or specific pixel dimensions.

-Centimeter-to-pixel conversion and vice-versa.

Displaying document photo dimension requirements.
Additional settings like automatic ZIP file creation and download.
Code Structure
The HTML file is divided into different sections to handle various aspects of the image conversion and manipulation process. Below are detailed descriptions of each part.

1. Head Section-

The head section of the HTML document includes meta tags, links to CSS files, and external resources:

charset="UTF-8": Ensures the document uses UTF-8 encoding.
viewport settings for mobile responsiveness.
Links to custom stylesheets and Google Fonts.
Manifest and theme color settings for a better mobile experience.
2. Image Conversion Options-

This section provides options for converting images into different formats (JPEG, PNG, WebP) and adjusting the output quality:

Output Format Selection: Three buttons to choose the desired format.
Output Quality Slider: Allows users to set the quality of the converted image from 0% to 100%.

3. Image Resizing Settings-

This part of the code provides users with options to resize images:

Users can select to resize using percentages or by specifying pixel dimensions.
Depending on the choice, the relevant input field (slider or number input) is displayed.

4. Centimeter to Pixel Converter-

This tool converts dimensions between centimeters and pixels:

Includes input fields for centimeters and pixels.
Uses a DPI (dots per inch) value of 96 for conversion.

5. Document Photo Dimensions Table-

Displays a table of common document photo requirements including:

Document name (e.g., Aadhaar Card, Passport).
Photo dimensions in both centimeters and inches.
Aspect ratio and additional requirements.

6. Progress Section-

Displays conversion progress information:

Shows a progress bar indicating the number of images converted.
Displays the current and total number of images.

7. Download Items Section-

Allows users to recover downloads in case of accidental cancellation or issues with the browser:

Provides a dropdown to select previously converted files.
Contains a download link for the selected item.

8. Dialogs for Theme, Privacy, and Open Source Information
The code includes dialogs for user customization and information:

Theme Dialog: Allows users to switch between different themes or customize the colors.
Privacy Dialog: Informs users about privacy practices and data handling.

Open Source Dialog: Lists open-source libraries used in the application, with licenses.

JavaScript Functionality-

The JavaScript functions handle interactions and data processing:

convertCmToPixel(): Converts centimeters to pixels using a DPI of 96.

clearFields(): Clears the input fields for conversion.

Dialog Managers: Handle opening and closing of theme and privacy dialogs.

Additional Features-

Offline Usage: The application can be installed as a Progressive Web App (PWA) for offline use.
No External Image Uploads: Image processing happens locally on the user's device, ensuring privacy.

Technologies and Libraries Used-

HTML & CSS: For structuring and styling the web page.
JavaScript: Handles user interaction, conversion logic, and dialog management.

Google Fonts: For typography.

JSZip, heic2any, UTIF.js: Open-source libraries used for handling image conversion.

External Resources-

The source code for this tool is available on GitHub.
Fonts are sourced from Google Fonts, and external libraries are fetched via JSDelivr.

User Interface Elements
The UI includes:

Buttons and Sliders: For easy selection of formats, quality settings, and conversions.
Responsive Design: Adapts to different screen sizes for a better user experience on mobile devices.

This documentation explains the HTML structure, JavaScript functionality, and Service Worker implementation of the image converter web application. The goal of this application is to enable users to convert and compress images, resize them, and handle image files with various configurations.

Table of Contents-
1.HTML Structure

2.Document Header

3.User Interface Elements

4.Image Conversion Options

5.Progress and Download Management

6.Table of Document Photo Dimensions

7.Pixel to Centimeter Converter

8.JavaScript Code

9.Conversion Functionality

10.Event Handling for Image Operations

11.Service Worker Implementation

12.Caching Strategies

13.Service Worker Events

14.External Resources and Libraries

1. HTML Structure

The HTML file is structured to create an intuitive user interface for image conversion and manipulation. It consists of several sections that allow users to interact with the application.

1.1 Document Header

The header contains meta tags for setting the character encoding, viewport properties, and theme colors.

It also includes links to external CSS stylesheets, Google Fonts, and the web app manifest for providing metadata about the web application.

1.2 User Interface Elements
Introduction Section: Displays the app's logo and a brief description of its functionality.

Image Conversion Options: 
Allows users to choose the output file format (JPEG, PNG, WebP) and the quality of the converted image.

1.3 Image Conversion Options
Output Quality Selection: Provides a slider to adjust the quality of the image between 0% and 100%.

Resize Options: Users can resize images by percentage, height, or width.

ZIP File Options:
 Allows the user to choose whether the converted images should be automatically zipped for download.

1.4 Progress and Download Management
Progress Indicator:
 Shows the conversion progress with the number of images converted out of the total.

Download Items: Allows users to recover downloads that might have been blocked or interrupted.

1.5 Table of Document Photo Dimensions
Displays a table of common document photo sizes with details about the required dimensions in centimeters, inches, aspect ratios, and additional requirements.

1.6 Pixel to Centimeter Converter
Includes an interactive converter for converting measurements between centimeters and pixels, using a standard DPI of 96.

2. JavaScript Code
The JavaScript file (convert.js) handles the main logic for the image conversion and interaction with the user interface.

2.1 Conversion Functionality convertCmToPixel Function: 

Converts centimeter measurements to pixels and vice versa based on the provided values and DPI of 96.

clearFields Function: Clears the input fields for both centimeter and pixel values.

2.2 Event Handling for Image Operations

Various buttons and event listeners are set up to manage file selection, image conversion, and download operations.

Progress Handling:
 Uses progress indicators to provide feedback to the user as images are being processed.

3. Service Worker Implementation
The Service Worker code is responsible for caching the application's assets and enabling offline functionality.

3.1 Caching Strategies-

Cache Name: The cache is named imageconverter-cache.

Files to Cache: A list of essential files that will be stored in the cache for offline usage, including HTML, CSS, JS files, external libraries, and icons.

javascript code:-
const cacheName = 'imageconverter-cache';
const filestoCache = [
    `./`,
    `./index.html`,
    `./convert.js`,
    `./heic2any.js`,
    `./manifest.json`,
    `./style.css`,
    `./icon.png`,
    'https://fonts.googleapis.com/css2?family=Montserrat:wght@700&family=Work+Sans&display=swap',
    `https://cdn.jsdelivr.net/npm/jszip@3.10.1/dist/jszip.min.js`,
    `https://cdn.jsdelivr.net/npm/utif@3.1.0/UTIF.min.js`,
];
3.2 Service Worker Events
Install Event: Caches the essential files when the service worker is installed.

javascript code :-
self.addEventListener('install', e => {
    e.waitUntil(
        caches.open(cacheName)
            .then(cache => cache.addAll(filestoCache))
    );
});
Activate Event: Claims control of all clients immediately without waiting.

javascript code :-

self.addEventListener('activate', e => self.clients.claim());

Fetch Event: Handles network requests by trying to fetch from the network first. If the request fails, it serves the cached version.

javascript code :-

self.addEventListener('fetch', event => {
    const req = event.request;
    if (req.url.indexOf("updatecode") !== -1) return fetch(req); else event.respondWith(networkFirst(req));
});

async function networkFirst(req) {
    try {
        const networkResponse = await fetch(req);
            const cache = await caches.open('imageconverter-cache');
            await cache.delete(req);
            await cache.put(req, networkResponse.clone());
        return networkResponse;
    } catch (error) {
        const cachedResponse = await caches.match(req);
        return cachedResponse;
    }
}

Explanation:

Network First Strategy: This approach attempts to get a fresh response from the network. If the network fails, it serves the cached response to ensure the application works offline.

4. External Resources and Libraries-

Google Fonts: Used to style the application with Montserrat and Work Sans fonts.

JSZip Library: Facilitates the creation of ZIP files for batch image downloads.

UTIF.js Library: Handles the conversion of TIFF images for better compatibility.
## 🔗 Links
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/mayur-katre-5117601b8/)


