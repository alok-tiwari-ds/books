## 🏛️ System Architecture & Technology Stack

Here’s a breakdown of the components and the recommended free, open-source libraries to build a local, web-based book scanning application. The core idea is to use a lightweight Python web framework to serve a modern JavaScript frontend, allowing for a rich user interface without needing a complex server setup.

### **Frontend (User Interface)**

The frontend is what the user sees and interacts with. It runs in a local web view and is responsible for displaying the bookshelf image and handling user input for segmenting book spines.

* **Framework:** **Eel**. This library is perfect for creating simple, offline HTML/JavaScript GUIs for Python scripts. It establishes a direct communication channel between the Python backend and the JavaScript frontend.
* **Image Interaction:** **Fabric.js** or **Konva.js**. These are powerful JavaScript libraries that work with the HTML `<canvas>` element. They make it easy to draw shapes (like the quadrilaterals for book spines) on top of an image and allow users to move the corners of those shapes with their mouse.

### **Backend (Python Core Logic)**

The backend runs all the heavy-lifting tasks. It processes images, runs the machine learning models, interacts with the database, and serves the frontend.

* **Image Processing:** **OpenCV-Python** (`opencv-python`). This is the essential library for all computer vision tasks. It will be used for:
    * Loading and saving images.
    * Detecting edges and lines for automated segmentation.
    * Performing perspective transforms to de-skew and normalize the book spine segments.
* **OCR Engine:** **Tesseract** (via the `pytesseract` wrapper). A solid, open-source OCR engine developed by Google. It's a great starting point for text extraction. For higher accuracy, **EasyOCR** is a more modern, deep-learning-based alternative.
* **Database:** **SQLite**. Included in Python's standard library (`sqlite3`), SQLite is a serverless, self-contained database engine. It stores the entire database in a single file, making it ideal for a local application.
* **Text Matching:** **TheFuzz** (`thefuzz` library). This library is excellent for string similarity matching. It will be used to compare the text extracted by the OCR with the book titles in your local database to find the most likely match.
* **Machine Learning & Training:** **PyTorch** or **TensorFlow/Keras**. These are the leading deep learning frameworks. You will use one of them to build, train, and deploy custom models for segmentation and potentially for improving OCR accuracy over time.

### **Local Book Database**

To identify books, the system needs a local, searchable catalog of book titles and authors.

* **Data Source:** **Open Library Data Dumps**. Open Library provides free, comprehensive dumps of their entire catalog. You can download these files, write a script to parse them for the necessary information (titles, authors), and load them into your SQLite database for fast, local searching.
* **Alternative Source:** The **Book-Crossing Dataset**. This dataset contains ISBNs, titles, and authors from a book-sharing website. It's another valuable source for building your local library.
