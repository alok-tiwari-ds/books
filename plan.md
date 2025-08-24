## 🗓️ Multi-Week Project Plan

This plan breaks the project into two main phases. Phase 1 focuses on building a functional Minimum Viable Product (MVP) where the user performs key steps manually. Phase 2 introduces automation and the machine learning feedback loop.

---

### **Phase 1: Building the Core Application (Weeks 1-4)**

**🎯 Goal:** Create a fully functional system where the user manually segments the books and corrects all information. This establishes the core workflow.

* **Week 1: Project Setup & UI Shell**
    1.  **Environment:** Set up a Python virtual environment. Install initial packages: `eel`, `opencv-python`, `numpy`.
    2.  **Basic App:** Create a "Hello World" application with Eel to confirm that the Python backend can communicate with the HTML/JavaScript frontend.
    3.  **UI Layout:** Design the main HTML page. It should include a large area for the bookshelf image, a sidebar or section to display results, and buttons for "Load Image" and "Save Shelf".
    4.  **Database Schema:** Write a Python script using the `sqlite3` module to create your local database file (`books.db`). Define the necessary tables: `shelves` (shelf_id, name) and `books` (book_id, shelf_id, position_index, title, author, original_image_path, segment_image_path, segment_coords).

* **Week 2: Image Loading & Manual Segmentation**
    1.  **File Loading:** Implement the "Load Image" button. This should open a file dialog in Python and load the selected image using OpenCV. The image data should then be passed to the JavaScript frontend for display.
    2.  **Interactive Canvas:** Integrate a library like Fabric.js into the frontend. When an image is loaded, display it on the canvas.
    3.  **Drawing & Editing Shapes:** Write the JavaScript code to allow a user to click four points on the canvas to draw a quadrilateral. The corners of this shape must be draggable so the user can adjust the segment.
    4.  **Data Transfer:** Create a "Process Segments" button. When clicked, it should send the coordinates of all user-drawn quadrilaterals from JavaScript back to the Python backend.

* **Week 3: Segment Processing & OCR**
    1.  **Perspective Transform:** In Python, write a function that takes an image and a set of four coordinates. Use OpenCV's `cv2.getPerspectiveTransform` and `cv2.warpPerspective` functions to transform the segmented area into a standardized, rectangular image of the book spine.
    2.  **OCR Integration:** Install `pytesseract` and the Tesseract engine. Write a Python function that takes a standardized segment image, performs OCR, and returns the extracted text.
    3.  **Display Results:** Pass the raw OCR text for each segment back to the frontend and display it next to a thumbnail of the corresponding standardized spine.

* **Week 4: Book Database & Title Matching**
    1.  **Database Population:** Download a data dump from Open Library. Write a separate, one-time Python script to parse this file and populate a `library` table in your SQLite database with book titles and authors.
    2.  **Fuzzy Matching:** Install `thefuzz`. For each OCR result, use `thefuzz` to query your `library` table and find the best matching title.
    3.  **User Correction UI:** For each book, display the OCR'd text, the best database match, and input fields for the title and author. The user must be able to accept the match, manually edit the text, or skip the book.

---

### **Phase 2: Automation & Machine Learning (Weeks 5-8+)**

**🎯 Goal:** Automate the manual steps using computer vision and build the infrastructure for the system to learn from user corrections.

* **Week 5: Automated Segmentation (V1)**
    1.  **Classical CV Approach:** Before diving into deep learning, implement a simple automated segmenter using OpenCV. The steps would be: convert the image to grayscale, apply Canny edge detection to find edges, and use the Hough Line Transform to detect strong vertical lines (the edges of books).
    2.  **Propose Segments:** Use the detected lines to generate proposed rectangular segments. Send these coordinates to the frontend automatically after an image is loaded. The user can then quickly correct them instead of drawing from scratch.

* **Week 6: Data Collection & Storage**
    1.  **Training Data Pipeline:** When the user clicks the final "Save Shelf" button, this is your opportunity to collect high-quality training data.
    2.  **Save Logic:** For each book, save the following:
        * **For Segmentation Training:** The original shelf image and the user's final, corrected coordinates for that book.
        * **For OCR Training:** The standardized, de-skewed spine image and the user's final, corrected title and author text.
    3.  **Database Finalization:** Store all the final, user-verified book information (title, author, position, etc.) in your `books` SQLite table.

* **Week 7: Building the Training Pipeline**
    1.  **"Refresh Model" UI:** Add a "Settings" page to your UI with a button like "Train New Segmentation Model".
    2.  **Training Script:** Create a `train.py` script. This script will query your database to pull all the verified segmentation data (images and their corresponding coordinates).
    3.  **Model Training:** Use PyTorch or TensorFlow to train a simple object detection model (e.g., a custom CNN or a fine-tuned MobileNet/YOLO) on this data.
    4.  **Save Model:** Save the trained model's weights to a versioned file (e.g., `segmentation_model_v2.h5`).

* **Week 8 & Beyond: Closing the Loop**
    1.  **Model Management:** Allow the user to select which version of the segmentation model they want to use from a dropdown in the Settings UI.
    2.  **Integrate Custom Model:** Modify the automated segmentation logic from Week 5. Instead of using the OpenCV line detection, load the user-selected ML model and use it to predict the book bounding boxes.
    3.  **Iterate and Improve:** Apply the same "collect data -> train -> deploy" loop for other components. You could fine-tune an OCR model on your collected spine images to improve its accuracy on your specific type of data.
