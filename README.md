# Spec-Tool
Interactive Spec software
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gehl Tech Interface</title>
  <!-- Load jsPDF from CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <!-- Load fonts from Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Bangers&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: 'Roboto', Arial, sans-serif;
      background: #fff;
      color: #000;
      min-height: 100vh;
      overflow-x: hidden;
      max-width: 100vw;
    }

    .navbar {
      display: flex;
      justify-content: center;
      background: rgba(255, 255, 255, 0.1);
      padding: 15px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.2);
      backdrop-filter: blur(10px);
    }

    .navbar button {
      margin: 0 15px;
      padding: 8px 16px;
      background: #fff;
      border: 1px solid #000;
      border-radius: 25px;
      color: #000;
      cursor: pointer;
      transition: all 0.3s;
    }

    .navbar button:hover {
      background: #000;
      color: #fff;
      transform: translateY(-2px);
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
    }

    .navbar button.active {
      background: #000;
      color: #fff;
    }

    .navbar button.active:hover {
      transform: translateY(0);
      box-shadow: none;
    }

    .page {
      display: none;
      padding: 30px;
      max-width: 1200px;
      margin: 0 auto;
    }

    h1 {
      text-align: center;
      color: #000;
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 2px;
      font-size: 2.5em;
    }

    h2 {
      color: #000;
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 2px;
      font-size: 2em;
      margin-bottom: 15px;
      display: inline-block;
    }

    h2.editable {
      padding: 5px 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      background: #fff;
      cursor: text;
      transition: border-color 0.3s;
    }

    h2.editable:hover, h2.editable:focus {
      border-color: #000;
      outline: none;
    }

    .uneditable-heading {
      color: #000;
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 2px;
      font-size: 2em;
      margin-bottom: 15px;
    }

    .experience-title {
      text-align: center;
      color: #000;
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 2px;
      font-size: 2em;
      margin-bottom: 20px;
    }

    .welcome-text {
      text-align: center;
      color: #000;
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 1.4em;
      margin-bottom: 30px;
      line-height: 1.5;
    }

    .section-header {
      display: flex;
      align-items: baseline;
      margin-bottom: 15px;
    }

    .section-number {
      color: #000;
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 2px;
      font-size: 2em;
      margin-right: 10px;
      line-height: 1;
    }

    .business-section, .client-section {
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 10px;
      padding: 20px;
      margin-bottom: 25px;
      transition: all 0.3s;
    }

    .business-section:hover {
      background: rgba(255, 255, 255, 0.08);
    }

    input[type="text"], textarea {
      width: 70%;
      max-width: 400px;
      padding: 10px;
      margin: 8px 0;
      background: #fff;
      border: 1px solid #000;
      border-radius: 5px;
      color: #000;
      outline: 2px solid #000;
    }

    button {
      padding: 10px 20px;
      background: #fff;
      border: 1px solid #000;
      border-radius: 25px;
      color: #000;
      cursor: pointer;
      transition: all 0.3s;
    }

    button:hover {
      background: #000;
      color: #fff;
      transform: translateY(-2px);
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
    }

    .business-images, .client-images-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 15px;
      margin: 20px 0;
    }

    .business-images .image-container, .client-images-grid .image-container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .business-images img, .client-images-grid img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 8px;
      border: 2px solid rgba(255, 255, 255, 0.1);
      transition: all 0.3s;
    }

    .client-images-grid img {
      height: 300px;
    }

    .client-images-grid img:hover {
      transform: scale(1.05);
      border-color: #ff0000;
    }

    .add-section-btn {
      display: block;
      margin: 20px auto;
      background: #fff;
      border: 1px solid #000;
    }

    .add-section-btn:hover {
      background: #000;
      color: #fff;
    }

    .remove-section-btn, .add-images-btn {
      display: inline-block;
      margin: 10px 5px;
      padding: 10px 20px;
      background: #fff;
      border: 1px solid #000;
      border-radius: 25px;
      color: #000;
      cursor: pointer;
      transition: all 0.3s;
    }

    .remove-section-btn:hover, .add-images-btn:hover {
      background: #000;
      color: #fff;
      transform: translateY(-2px);
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
    }

    input[type="file"] {
      display: none;
    }

    .image-container {
      position: relative;
      width: 100%;
    }

    .remove-image-btn {
      position: absolute;
      top: 5px;
      right: 5px;
      padding: 5px 10px;
      background: rgba(255, 255, 255, 0.8);
      border: 1px solid #000;
      border-radius: 15px;
      color: #000;
      font-size: 12px;
      cursor: pointer;
      transition: all 0.3s;
    }

    .remove-image-btn:hover {
      background: #000;
      color: #fff;
      transform: translateY(-2px);
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
    }

    .toggle-container {
      text-align: center;
      margin-bottom: 20px;
    }

    .toggle-label {
      color: #000;
      font-family: 'Roboto', Arial, sans-serif;
      margin-right: 10px;
    }

    .toggle-switch {
      position: relative;
      display: inline-block;
      width: 60px;
      height: 34px;
    }

    .toggle-switch input {
      opacity: 0;
      width: 0;
      height: 0;
    }

    .slider {
      position: absolute;
      cursor: pointer;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: #fff;
      border: 1px solid #000;
      border-radius: 34px;
      transition: 0.4s;
    }

    .slider:before {
      position: absolute;
      content: "";
      height: 26px;
      width: 26px;
      left: 4px;
      bottom: 4px;
      background-color: #000;
      border-radius: 50%;
      transition: 0.4s;
    }

    input:checked + .slider {
      background-color: #000;
    }

    input:checked + .slider:before {
      transform: translateX(26px);
      background-color: #fff;
    }

    .image-name {
      text-align: center;
      color: #000;
      font-family: 'Bangers', cursive;
      font-size: 1.2em;
      margin: 5px 0;
    }

    .add-field-btn {
      display: block;
      margin: 5px auto;
      padding: 5px 10px;
      background: #fff;
      border: 1px solid #000;
      border-radius: 15px;
      color: #000;
      font-size: 12px;
      cursor: pointer;
      transition: all 0.3s;
    }

    .add-field-btn:hover {
      background: #000;
      color: #fff;
      transform: translateY(-2px);
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
    }

    .custom-field {
      display: block;
      width: 80%;
      margin: 3px auto;
      padding: 3px;
      background: #fff;
      border: 1px solid #000;
      border-radius: 5px;
      color: #000;
      font-family: 'Roboto', Arial, sans-serif;
      font-size: 12px;
      text-align: center;
    }

    .custom-field::placeholder {
      color: #666;
    }

    .save-view-container {
      text-align: center;
      margin-top: 15px;
    }

    .save-view-container p {
      color: #000;
      margin-bottom: 10px;
    }

    .link-buttons {
      margin-top: 10px;
      display: none;
    }

    .spinner {
      display: none;
      margin: 10px auto;
      border: 4px solid rgba(0, 0, 0, 0.1);
      border-left-color: #000;
      border-radius: 50%;
      width: 24px;
      height: 24px;
      animation: spin 1s linear infinite;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    .results-table {
      width: 100%;
      max-width: 800px;
      margin: 20px auto;
      border-collapse: collapse;
      overflow-x: auto;
    }

    .results-table th, .results-table td {
      padding: 10px;
      border: 1px solid rgba(0, 0, 0, 0.1);
      text-align: left;
      color: #000;
    }

    .results-table th {
      background: rgba(0, 0, 0, 0.1);
      font-weight: bold;
    }

    .saved-experience-btn {
      margin: 5px;
      padding: 5px 10px;
      background: #fff;
      border: 1px solid #000;
      border-radius: 15px;
      color: #000;
      cursor: pointer;
      transition: all 0.3s;
    }

    .saved-experience-btn:hover {
      background: #000;
      color: #fff;
      transform: translateY(-2px);
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
    }

    .rename-input {
      width: 200px;
      padding: 5px;
      margin-left: 10px;
      font-size: 1em;
    }

    /* Popup Styles */
    .popup {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }

    .popup-content {
      background: #fff;
      border: 1px solid #000;
      border-radius: 10px;
      padding: 20px;
      text-align: center;
      max-width: 400px;
      width: 80%;
    }

    .popup-content p {
      color: #000;
      font-family: 'Roboto', Arial, sans-serif;
      margin-bottom: 20px;
    }

    .popup-content input, .popup-content .link-text {
      width: 80%;
      padding: 8px;
      margin-bottom: 10px;
      border: 1px solid #000;
      border-radius: 5px;
      color: #000;
      background: #fff;
      font-family: 'Roboto', Arial, sans-serif;
      font-size: 14px;
      word-break: break-all;
    }

    .popup-content button {
      margin: 0 10px;
    }

    .error-message {
      color: #ff0000;
      font-family: 'Roboto', Arial, sans-serif;
      font-size: 14px;
      margin-bottom: 10px;
      display: none;
    }

    /* Mobile Responsiveness */
    @media (max-width: 768px) {
      .navbar {
        flex-direction: column;
        align-items: center;
        padding: 10px;
      }

      .navbar button {
        margin: 5px 0;
        width: 80%;
        padding: 10px;
        font-size: 1em;
      }

      .page {
        padding: 15px;
      }

      h1 {
        font-size: 1.8em;
      }

      h2, .uneditable-heading, .experience-title {
        font-size: 1.5em;
      }

      .welcome-text {
        font-size: 1.2em;
      }

      .business-images, .client-images-grid {
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
        gap: 10px;
      }

      .business-images img, .client-images-grid img {
        height: 150px;
      }

      .client-images-grid img {
        height: 200px;
      }

      .section-number {
        font-size: 1.5em;
      }

      .remove-section-btn, .add-images-btn, .add-section-btn, button {
        padding: 8px 15px;
        font-size: 0.9em;
      }

      .popup-content {
        width: 90%;
        max-width: 300px;
        padding: 15px;
      }

      .popup-content input, .popup-content .link-text {
        width: 100%;
        font-size: 12px;
      }

      .popup-content button {
        padding: 8px 15px;
        font-size: 0.9em;
      }

      .results-table th, .results-table td {
        padding: 8px;
        font-size: 0.9em;
      }

      .saved-experience-btn {
        padding: 5px 8px;
        font-size: 0.8em;
      }
    }

    @media (max-width: 480px) {
      h1 {
        font-size: 1.5em;
      }

      h2, .uneditable-heading, .experience-title {
        font-size: 1.2em;
      }

      .welcome-text {
        font-size: 1em;
      }

      .business-images, .client-images-grid {
        grid-template-columns: 1fr;
        gap: 8px;
      }

      .business-images img, .client-images-grid img {
        height: 120px;
      }

      .client-images-grid img {
        height: 160px;
      }

      .section-number {
        font-size: 1.2em;
      }

      .remove-section-btn, .add-images-btn, .add-section-btn, button {
        padding: 6px 12px;
        font-size: 0.8em;
      }

      .popup-content {
        width: 95%;
        max-width: 250px;
        padding: 10px;
      }

      .popup-content input, .popup-content .link-text {
        font-size: 10px;
      }

      .popup-content button {
        padding: 6px 10px;
        font-size: 0.8em;
      }
    }
  </style>
</head>
<body>
  <!-- Navigation Bar -->
  <div class="navbar" id="navbar">
    <button id="nav-business" onclick="showPage('business')">Experience Builder</button>
    <button id="nav-admin" onclick="showPage('admin')">Admin</button>
  </div>

  <!-- Business Interface Page -->
  <div id="business-page" class="page">
    <h1>Experience Builder</h1>
    <div class="toggle-container">
      <label class="toggle-label" for="numbering-toggle">Show Numbering and Image Names</label>
      <label class="toggle-switch">
        <input type="checkbox" id="numbering-toggle" onchange="toggleNumbering()">
        <span class="slider"></span>
      </label>
    </div>
    <h2 id="business-experience-title" class="experience-title" style="display: none;"></h2>
    <div id="business-content">
      <!-- Business content is rendered here -->
    </div>
    <button class="add-section-btn" onclick="addSection()">Add New Section</button>
    <div class="save-view-container">
      <button onclick="saveView()">Generate Client Link</button>
      <div id="link-spinner" class="spinner"></div>
      <div id="link-buttons" class="link-buttons">
        <button onclick="showLinkPopup('copy')">Copy Link</button>
        <button onclick="showLinkPopup('preview')">Preview Link</button>
      </div>
    </div>
    <div style="text-align: center; margin-top: 20px;">
      <button onclick="saveExperience()">Save Experience</button>
      <button onclick="createNewExperience()">Create New Experience</button>
    </div>

    <!-- Popup for Creating New Experience -->
    <div id="new-experience-popup" class="popup">
      <div class="popup-content">
        <p>Do you want to save this experience?</p>
        <button id="save-yes-btn">Yes</button>
        <button onclick="discardAndReset()">No</button>
      </div>
    </div>

    <!-- Popup for Naming and Saving Experience -->
    <div id="save-name-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p class="error-message" id="name-error">Try again, this name is already in use</p>
        <p>Enter a name for this experience:</p>
        <input type="text" id="experience-name" placeholder="Experience Name">
        <button onclick="saveNamedExperience()">Save</button>
        <button onclick="hidePopup('save-name-popup')">Cancel</button>
      </div>
    </div>

    <!-- Popup for Duplicating Experience -->
    <div id="duplicate-experience-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p class="error-message" id="duplicate-name-error">Try again, this name is already in use</p>
        <p>Enter a name for the duplicated experience:</p>
        <input type="text" id="duplicate-experience-name" placeholder="New Experience Name">
        <button onclick="createDuplicatedExperience()">Create</button>
        <button onclick="hidePopup('duplicate-experience-popup')">Cancel</button>
      </div>
    </div>

    <!-- Popup for Save Changes Before Leaving -->
    <div id="save-changes-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p>Do you want to save your changes?</p>
        <button id="save-changes-yes-btn">Yes</button>
        <button onclick="discardAndNavigate()">No</button>
      </div>
    </div>

    <!-- Popup for Link Preview/Copy -->
    <div id="link-preview-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p>Client Link:</p>
        <p class="link-text" id="link-text"></p>
        <button onclick="copyLink()">Copy</button>
        <button onclick="hidePopup('link-preview-popup')">Close</button>
      </div>
    </div>

    <!-- Popup for Copy Link Success -->
    <div id="copy-link-success-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p>Link copied to clipboard!</p>
        <button onclick="hidePopup('copy-link-success-popup')">OK</button>
      </div>
    </div>
  </div>

  <!-- Client Preview / Client Experience Page -->
  <div id="client-page" class="page">
    <h1>Client Experience</h1>
    <h2 id="client-experience-title" class="experience-title" style="display: none;"></h2>
    <p class="welcome-text">This is an interactive Experience, select the items/images you like and at the end download your brochure that creates you a PDF with the items/images you selected.</p>
    <div id="client-content">
      <!-- Client content is rendered here -->
    </div>
    <div style="text-align: center; margin-top: 30px;">
      <button onclick="showDownloadPopup()">Download Your Brochure</button>
    </div>

    <!-- Popup for Download Brochure -->
    <div id="download-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p>Enter your email to download:</p>
        <input type="text" id="download-email" placeholder="Your Email" required>
        <button onclick="downloadPDF()">Download</button>
        <button onclick="hidePopup('download-popup')">Cancel</button>
      </div>
    </div>
  </div>

  <!-- Admin Page -->
  <div id="admin-page" class="page">
    <h1>Gehl Tech Admin</h1>
    <p style="text-align: center; color: #000;">View saved experiences and client submissions below.</p>
    <div id="admin-content">
      <!-- Admin content (saved experiences and client submissions) is rendered here -->
    </div>

    <!-- Popup for Deleting Experience -->
    <div id="delete-experience-popup" class="popup" style="display: none;">
      <div class="popup-content">
        <p>Are you sure you want to delete this experience?</p>
        <button id="confirm-delete">Yes</button>
        <button onclick="hidePopup('delete-experience-popup')">No</button>
      </div>
    </div>
  </div>

  <script>
    /******************************
     * Global Data & Initialization
     ******************************/
    let sections = [];
    const defaultSectionNames = [
      "Exteriors", 
      "Interiors", 
      "Kitchens", 
      "Amenities", 
      "Other"
    ];
    
    // Check if a client view is provided via URL query parameter
    function getQueryParam(param) {
      const params = new URLSearchParams(window.location.search);
      return params.get(param);
    }
    
    let isClientView = false;
    const experienceIdParam = getQueryParam("experienceId");
    if (experienceIdParam) {
      try {
        const storedExperiences = JSON.parse(localStorage.getItem("tempExperiences") || "{}");
        if (storedExperiences[experienceIdParam]) {
          sections = storedExperiences[experienceIdParam].sections;
          isClientView = true;
        }
      } catch(e) {
        console.error("Error loading experience from storage.", e);
      }
    }

    // Set to keep track of selected images on the client page.
    let selectedImages = new Set();
    let showNumbering = false;
    // Clear localStorage and initialize empty arrays on page load
    localStorage.removeItem("savedExperiences");
    localStorage.removeItem("analyticsData");
    let savedExperiences = [];
    let analyticsData = [];
    let editingExperienceId = null; // Track if we're editing an existing experience
    let experienceToDelete = null; // Track which experience to delete
    let currentExperienceName = null; // Track the current experience name
    let lastSavedSections = JSON.stringify(sections); // Track the last saved state
    let duplicatingExperienceIndex = null; // Track which experience is being duplicated
    let generatedLink = ""; // Store the generated link
    let currentPage = 'business'; // Track current page
    let targetPage = null; // Track page to navigate to after save prompt

    /*****************************
     * Helper: Read File as Data *
     *****************************/
    function readFileAsDataURL(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = function(e) {
          resolve(e.target.result);
        };
        reader.onerror = function(err) {
          reject(err);
        };
        reader.readAsDataURL(file);
      });
    }
    
    /************************
     * Helper: Compare Sections
     ************************/
    function hasUnsavedChanges() {
      const currentSections = JSON.stringify(sections);
      return currentSections !== lastSavedSections;
    }

    /************************
     * Helper: Close All Popups
     ************************/
    function closeAllPopups() {
      const popups = [
        "new-experience-popup",
        "save-name-popup",
        "duplicate-experience-popup",
        "save-changes-popup",
        "link-preview-popup",
        "copy-link-success-popup",
        "download-popup",
        "delete-experience-popup"
      ];
      popups.forEach(popupId => {
        const popup = document.getElementById(popupId);
        if (popup) popup.style.display = "none";
      });
    }

    /********************
     * Rendering Views  *
     ********************/
    function toggleNumbering() {
      showNumbering = document.getElementById("numbering-toggle").checked;
      renderBusiness();
      renderClient();
    }

    function renderBusiness() {
      const businessContainer = document.getElementById("business-content");
      businessContainer.innerHTML = "";
      const experienceTitle = document.getElementById("business-experience-title");
      if (currentExperienceName) {
        experienceTitle.textContent = currentExperienceName;
        experienceTitle.style.display = "block";
      } else {
        experienceTitle.style.display = "none";
      }

      if (sections.length === 0) {
        businessContainer.innerHTML = "<p style='text-align: center; color: #000;'>No sections yet. Click 'Add New Section' to start.</p>";
        return;
      }

      sections.forEach((section, secIndex) => {
        const sectionDiv = document.createElement("div");
        sectionDiv.className = "business-section";
        
        const headerDiv = document.createElement("div");
        headerDiv.className = "section-header";
        if (showNumbering) {
          const sectionNumber = document.createElement("span");
          sectionNumber.className = "section-number";
          sectionNumber.textContent = `${secIndex + 1}`;
          headerDiv.appendChild(sectionNumber);
        }
        const header = document.createElement("h2");
        header.className = "editable";
        header.textContent = section.name;
        header.contentEditable = true;
        header.onblur = function() {
          section.name = header.textContent.trim() || "Name";
          renderBusiness();
          renderClient();
        };
        header.onkeypress = function(e) {
          if (e.key === "Enter") {
            e.preventDefault();
            header.blur();
          }
        };
        headerDiv.appendChild(header);
        sectionDiv.appendChild(headerDiv);
        
        const imagesDiv = document.createElement("div");
        imagesDiv.className = "business-images";
        section.images.forEach((img, imgIndex) => {
          const imageContainer = document.createElement("div");
          imageContainer.className = "image-container";

          const imgWrapper = document.createElement("div");
          imgWrapper.style.position = "relative";
          const imgElem = document.createElement("img");
          imgElem.src = img.url;
          imgElem.alt = `Image ${img.id}`;
          imgWrapper.appendChild(imgElem);

          const removeImageButton = document.createElement("button");
          removeImageButton.className = "remove-image-btn";
          removeImageButton.textContent = "Remove";
          removeImageButton.onclick = function() {
            sections[secIndex].images.splice(imgIndex, 1);
            renderBusiness();
          };
          imgWrapper.appendChild(removeImageButton);

          imageContainer.appendChild(imgWrapper);

          if (showNumbering) {
            const imageName = document.createElement("div");
            imageName.className = "image-name";
            const letter = String.fromCharCode(65 + imgIndex);
            imageName.textContent = `${secIndex + 1}${letter}`;
            imageContainer.appendChild(imageName);

            const addFieldButton = document.createElement("button");
            addFieldButton.className = "add-field-btn";
            addFieldButton.textContent = "Add Field";
            addFieldButton.onclick = function() {
              if (!img.fields) img.fields = [];
              img.fields.push("");
              renderBusiness();
            };
            imageContainer.appendChild(addFieldButton);

            img.fields.forEach((field, fieldIndex) => {
              const fieldInput = document.createElement("input");
              fieldInput.type = "text";
              fieldInput.className = "custom-field";
              fieldInput.value = field || "";
              fieldInput.placeholder = "Enter pricing, spec, or product number";
              fieldInput.onchange = function() {
                img.fields[fieldIndex] = fieldInput.value;
              };
              imageContainer.appendChild(fieldInput);
            });
          }

          imagesDiv.appendChild(imageContainer);
        });
        sectionDiv.appendChild(imagesDiv);
        
        const buttonContainer = document.createElement("div");
        buttonContainer.style.textAlign = "center";

        const fileInput = document.createElement("input");
        fileInput.type = "file";
        fileInput.accept = "image/jpeg, image/png";
        fileInput.multiple = true;
        fileInput.id = `file-input-${secIndex}`;
        fileInput.onchange = function(e) { uploadFiles(e, secIndex); };
        buttonContainer.appendChild(fileInput);

        const addImagesButton = document.createElement("button");
        addImagesButton.className = "add-images-btn";
        addImagesButton.textContent = "Add Images";
        addImagesButton.onclick = function(event) {
          event.preventDefault();
          event.stopPropagation();
          fileInput.click();
        };
        buttonContainer.appendChild(addImagesButton);

        const removeButton = document.createElement("button");
        removeButton.className = "remove-section-btn";
        removeButton.textContent = "Remove Section";
        removeButton.onclick = function() {
          sections.splice(secIndex, 1);
          renderBusiness();
        };
        buttonContainer.appendChild(removeButton);
        
        sectionDiv.appendChild(buttonContainer);
        businessContainer.appendChild(sectionDiv);
      });
    }
    
    function renderClient() {
      const clientContainer = document.getElementById("client-content");
      clientContainer.innerHTML = "";
      const experienceTitle = document.getElementById("client-experience-title");
      if (currentExperienceName) {
        experienceTitle.textContent = currentExperienceName;
        experienceTitle.style.display = "block";
        experienceTitle.removeAttribute("contentEditable");
      } else {
        experienceTitle.style.display = "none";
      }

      sections.forEach((section, secIndex) => {
        const sectionContainer = document.createElement("div");
        sectionContainer.className = "client-section";
        
        const headerDiv = document.createElement("div");
        headerDiv.className = "section-header";
        if (showNumbering) {
          const sectionNumber = document.createElement("span");
          sectionNumber.className = "section-number";
          sectionNumber.textContent = `${secIndex + 1}`;
          headerDiv.appendChild(sectionNumber);
        }
        const header = document.createElement("h2");
        header.textContent = section.name;
        headerDiv.appendChild(header);
        sectionContainer.appendChild(headerDiv);
        
        const grid = document.createElement("div");
        grid.className = "client-images-grid";
        section.images.forEach((img, imgIndex) => {
          const imageContainer = document.createElement("div");
          imageContainer.className = "image-container";
          
          const imgElem = document.createElement("img");
          imgElem.src = img.url;
          imgElem.alt = `Image ${img.id}`;
          imgElem.id = `sec${secIndex}-img${imgIndex}`;
          imgElem.onclick = function() {
            toggleSelection(secIndex, imgIndex, imgElem);
          };
          imageContainer.appendChild(imgElem);

          if (showNumbering) {
            const imageName = document.createElement("div");
            imageName.className = "image-name";
            const letter = String.fromCharCode(65 + imgIndex);
            imageName.textContent = `${secIndex + 1}${letter}`;
            imageContainer.appendChild(imageName);

            img.fields.forEach(field => {
              if (field) {
                const fieldDisplay = document.createElement("div");
                fieldDisplay.className = "custom-field";
                fieldDisplay.textContent = field;
                imageContainer.appendChild(fieldDisplay);
              }
            });
          }

          grid.appendChild(imageContainer);
        });
        sectionContainer.appendChild(grid);
        clientContainer.appendChild(sectionContainer);
      });
    }
    
    function renderAdmin() {
      const adminContainer = document.getElementById("admin-content");
      adminContainer.innerHTML = "";

      // Saved Experiences Section
      const savedExperiencesDiv = document.createElement("div");
      const savedHeading = document.createElement("h2");
      savedHeading.className = "uneditable-heading";
      savedHeading.textContent = "Saved Experiences";
      savedExperiencesDiv.appendChild(savedHeading);
      if (savedExperiences.length === 0) {
        savedExperiencesDiv.innerHTML += "<p style='color: #000; text-align: center;'>No saved experiences yet.</p>";
      } else {
        savedExperiences.forEach((exp, expIndex) => {
          const expDiv = document.createElement("div");
          expDiv.style = "margin-bottom: 15px; padding: 10px; background: rgba(255, 255, 255, 0.05); border-radius: 5px; display: flex; align-items: center;";
          const nameSpan = document.createElement("span");
          nameSpan.style = "color: #000; flex-grow: 1;";
          nameSpan.textContent = exp.name || `Experience ${expIndex + 1}`;
          nameSpan.contentEditable = true;
          nameSpan.onblur = function() {
            exp.name = nameSpan.textContent.trim() || `Experience ${expIndex + 1}`;
            localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
            renderAdmin();
          };
          nameSpan.onkeypress = function(e) {
            if (e.key === "Enter") {
              e.preventDefault();
              nameSpan.blur();
            }
          };
          expDiv.appendChild(nameSpan);

          const previewButton = document.createElement("button");
          previewButton.className = "saved-experience-btn";
          previewButton.textContent = "Preview Experience";
          previewButton.onclick = function() {
            const tempSections = JSON.parse(JSON.stringify(exp.sections));
            sections = tempSections;
            currentExperienceName = exp.name;
            showPage("client");
            renderClient();
          };
          expDiv.appendChild(previewButton);

          const editButton = document.createElement("button");
          editButton.className = "saved-experience-btn";
          editButton.textContent = "Edit Experience";
          editButton.onclick = function() {
            sections = JSON.parse(JSON.stringify(exp.sections));
            lastSavedSections = JSON.stringify(sections);
            currentExperienceName = exp.name;
            editingExperienceId = exp.id;
            showPage("business");
            renderBusiness();
          };
          expDiv.appendChild(editButton);

          const deleteButton = document.createElement("button");
          deleteButton.className = "saved-experience-btn";
          deleteButton.textContent = "Delete Experience";
          deleteButton.onclick = function() {
            experienceToDelete = expIndex;
            document.getElementById("delete-experience-popup").style.display = "flex";
            document.getElementById("confirm-delete").onclick = function() {
              savedExperiences.splice(experienceToDelete, 1);
              localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
              experienceToDelete = null;
              hidePopup('delete-experience-popup');
              renderAdmin();
            };
          };
          expDiv.appendChild(deleteButton);

          const duplicateButton = document.createElement("button");
          duplicateButton.className = "saved-experience-btn";
          duplicateButton.textContent = "Duplicate Experience";
          duplicateButton.onclick = function() {
            duplicatingExperienceIndex = expIndex;
            document.getElementById("duplicate-experience-popup").style.display = "flex";
            document.getElementById("duplicate-experience-name").value = `Copy of ${exp.name || `Experience ${expIndex + 1}`}`;
            document.getElementById("duplicate-name-error").style.display = "none";
          };
          expDiv.appendChild(duplicateButton);

          savedExperiencesDiv.appendChild(expDiv);
        });
      }
      adminContainer.appendChild(savedExperiencesDiv);

      // Client Submissions Section
      const clientSubmissionsDiv = document.createElement("div");
      const clientHeading = document.createElement("h2");
      clientHeading.className = "uneditable-heading";
      clientHeading.textContent = "Client Submissions";
      clientSubmissionsDiv.appendChild(clientHeading);
      if (analyticsData.length === 0) {
        clientSubmissionsDiv.innerHTML += "<p style='color: #000; text-align: center;'>No client submissions yet.</p>";
      } else {
        const table = document.createElement("table");
        table.className = "results-table";
        table.innerHTML = `
          <tr>
            <th>Email</th>
            <th>User's PDF</th>
          </tr>
        `;
        analyticsData.forEach(record => {
          const pdfCell = record.pdfBase64 
            ? `<button class="saved-experience-btn" onclick="downloadUserPDF('${record.id}')">Download PDF</button>`
            : "No PDF available";
          table.innerHTML += `
            <tr>
              <td>${record.email || "Anonymous"}</td>
              <td>${pdfCell}</td>
            </tr>
          `;
        });
        clientSubmissionsDiv.appendChild(table);
      }
      adminContainer.appendChild(clientSubmissionsDiv);
    }
    
    /************************
     * File Upload Handler  *
     ************************/
    async function uploadFiles(event, secIndex) {
      const files = event.target.files;
      if (!files.length) return;
      for (let file of files) {
        try {
          const dataUrl = await readFileAsDataURL(file);
          const newId = `${secIndex}-${sections[secIndex].images.length + 1}`;
          sections[secIndex].images.push({ id: newId, url: dataUrl, fields: [] });
        } catch (e) {
          console.error("Error reading file:", e);
        }
      }
      // Reset file input and update views.
      event.target.value = "";
      renderBusiness();
      renderClient();
    }
    
    /********************
     * Interaction Code *
     ********************/
    function toggleSelection(secIndex, imgIndex, imgElement) {
      const key = secIndex + "-" + imgIndex;
      if (selectedImages.has(key)) {
        selectedImages.delete(key);
        imgElement.style.border = "2px solid rgba(255, 255, 255, 0.1)";
      } else {
        selectedImages.add(key);
        imgElement.style.border = "2px solid #ff0000";
      }
    }
    
    // Read a displayed <img> into a base64 string for jsPDF
    function getBase64Image(imgEl) {
      const canvas = document.createElement('canvas');
      canvas.width  = imgEl.naturalWidth;
      canvas.height = imgEl.naturalHeight;
      canvas.getContext('2d').drawImage(imgEl, 0, 0);
      return canvas.toDataURL('image/png');
    }
    
    function showDownloadPopup() {
      if (selectedImages.size === 0) {
        alert("Please select at least one image.");
        return;
      }
      document.getElementById("download-popup").style.display = "flex";
      document.getElementById("download-email").value = "";
    }
    
    async function downloadPDF() {
      const emailInput = document.getElementById("download-email");
      const email = emailInput.value.trim();
      if (!email) {
        alert("Please enter an email address.");
        return;
      }
      hidePopup("download-popup");

      // Generate PDF
      const { jsPDF } = window.jspdf;
      const pdf = new jsPDF({ unit: "mm", format: "a4" });
      const pageW = pdf.internal.pageSize.getWidth() - 20; // 10mm margins
      let firstPage = true;
      let yOffset = 25;

      for (let sec = 0; sec < sections.length; sec++) {
        const keys = [...selectedImages].filter(k => k.startsWith(sec + "-"));
        if (keys.length === 0) continue;

        for (const key of keys) {
          const imgIdx = key.split("-")[1];
          const imgEl = document.getElementById(`sec${sec}-img${imgIdx}`);
          const dataURL = getBase64Image(imgEl);
          const ratio = imgEl.naturalHeight / imgEl.naturalWidth;
          const imgH = pageW * ratio;

          if (!firstPage) {
            pdf.addPage();
            yOffset = 25;
          }
          firstPage = false;

          // Section title
          pdf.setFontSize(16);
          pdf.text(sections[sec].name, 105, 15, { align: "center" });

          // Image
          pdf.addImage(dataURL, "PNG", 10, yOffset, pageW, imgH);
          yOffset += imgH + 10;

          // Add numbering and fields if toggled on
          if (showNumbering) {
            const letter = String.fromCharCode(65 + parseInt(imgIdx));
            const imageNumber = `${sec + 1}${letter}`;
            pdf.setFontSize(12);
            pdf.text(imageNumber, 10, yOffset);
            yOffset += 10;

            const imgData = sections[sec].images[imgIdx];
            if (imgData.fields && imgData.fields.length > 0) {
              imgData.fields.forEach(field => {
                if (field) {
                  pdf.text(field, 10, yOffset);
                  yOffset += 10;
                }
              });
            }
          }
        }
      }

      // Save PDF as Base64 for storage
      const pdfBase64 = pdf.output('datauristring');
      const pdfId = Date.now().toString(); // Unique ID for submission
      pdf.save("gehl-tech-brochure.pdf");

      // Store in analytics
      analyticsData.push({
        id: pdfId,
        email: email,
        count: selectedImages.size,
        pdfBase64: pdfBase64
      });
      localStorage.setItem("analyticsData", JSON.stringify(analyticsData));
      renderAdmin(); // Refresh Admin to show new submission
    }

    function downloadUserPDF(pdfId) {
      const record = analyticsData.find(rec => rec.id === pdfId);
      if (record && record.pdfBase64) {
        const link = document.createElement('a');
        link.href = record.pdfBase64;
        link.download = `client-brochure-${pdfId}.pdf`;
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      } else {
        alert("PDF not available for download.");
      }
    }
    
    /***********************
     * Generate Client Link *
     ***********************/
    function saveView() {
      const spinner = document.getElementById("link-spinner");
      const linkButtons = document.getElementById("link-buttons");
      spinner.style.display = "block";
      linkButtons.style.display = "none";
      setTimeout(() => {
        // Generate unique experience ID
        const experienceId = Date.now().toString();
        // Store experience in localStorage
        let storedExperiences = JSON.parse(localStorage.getItem("tempExperiences") || "{}");
        storedExperiences[experienceId] = { sections: JSON.parse(JSON.stringify(sections)) };
        localStorage.setItem("tempExperiences", JSON.stringify(storedExperiences));
        // Generate short link
        generatedLink = window.location.origin + window.location.pathname + '?experienceId=' + experienceId;
        spinner.style.display = "none";
        linkButtons.style.display = "block";
      }, 1000); // Simulate 1-second processing
    }

    function showLinkPopup(action) {
      if (!generatedLink) {
        alert("Please generate a client link first.");
        return;
      }
      closeAllPopups();
      const linkText = document.getElementById("link-text");
      linkText.textContent = generatedLink;
      document.getElementById("link-preview-popup").style.display = "flex";
      if (action === 'copy') {
        copyLink();
      }
    }

    function copyLink() {
      const linkText = document.getElementById("link-text");
      const range = document.createRange();
      range.selectNode(linkText);
      window.getSelection().removeAllRanges();
      window.getSelection().addRange(range);
      document.execCommand("copy");
      window.getSelection().removeAllRanges();
      hidePopup("link-preview-popup");
      document.getElementById("copy-link-success-popup").style.display = "flex";
    }

    /****************************
     * Create New Experience    *
     ****************************/
    function createNewExperience() {
      if (!hasUnsavedChanges()) {
        // No changes: start new experience immediately
        resetToNewExperience();
      } else if (currentExperienceName && editingExperienceId !== null) {
        // Named with changes: prompt to save
        closeAllPopups();
        document.getElementById("new-experience-popup").style.display = "flex";
        const saveYesBtn = document.getElementById("save-yes-btn");
        saveYesBtn.onclick = function() {
          // Save the named experience
          const index = savedExperiences.findIndex(exp => exp.id === editingExperienceId);
          if (index !== -1) {
            savedExperiences[index] = {
              id: editingExperienceId,
              name: currentExperienceName,
              sections: JSON.parse(JSON.stringify(sections))
            };
            localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
            lastSavedSections = JSON.stringify(sections);
            hidePopup("new-experience-popup");
            resetToNewExperience();
            renderAdmin();
          }
        };
      } else {
        // Unnamed with changes: prompt to save and name
        closeAllPopups();
        document.getElementById("new-experience-popup").style.display = "flex";
        const saveYesBtn = document.getElementById("save-yes-btn");
        saveYesBtn.onclick = function() {
          hidePopup("new-experience-popup");
          closeAllPopups();
          document.getElementById("save-name-popup").style.display = "flex";
          document.getElementById("experience-name").value = "";
          document.getElementById("name-error").style.display = "none";
        };
      }
    }

    function saveNamedExperience() {
      const name = document.getElementById("experience-name").value.trim();
      if (!name) {
        document.getElementById("name-error").textContent = "Please enter a name";
        document.getElementById("name-error").style.display = "block";
        return;
      }
      // Check for duplicate name
      if (savedExperiences.some(exp => exp.name === name)) {
        document.getElementById("name-error").textContent = "Try again, this name is already in use";
        document.getElementById("name-error").style.display = "block";
        return;
      }
      const newExperience = {
        id: savedExperiences.length,
        name: name,
        sections: JSON.parse(JSON.stringify(sections))
      };
      savedExperiences.push(newExperience);
      localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
      currentExperienceName = name;
      editingExperienceId = newExperience.id;
      lastSavedSections = JSON.stringify(sections);
      hidePopup("save-name-popup");
      closeAllPopups();
      // If navigating from save-changes-popup, go to target page
      if (targetPage) {
        resetToNewExperience();
        showPage(targetPage);
      }
      renderBusiness();
      renderAdmin();
    }

    function discardAndReset() {
      hidePopup("new-experience-popup");
      resetToNewExperience();
    }

    function resetToNewExperience() {
      sections = [];
      currentExperienceName = null;
      editingExperienceId = null;
      lastSavedSections = JSON.stringify(sections);
      generatedLink = "";
      document.getElementById("link-buttons").style.display = "none";
      document.getElementById("link-spinner").style.display = "none";
      renderBusiness();
      // Removed showPage('business') to prevent navigation loop
    }

    /****************************
     * Save Experience          *
     ****************************/
    function saveExperience() {
      if (currentExperienceName && editingExperienceId !== null) {
        // Update existing named experience
        const index = savedExperiences.findIndex(exp => exp.id === editingExperienceId);
        if (index !== -1) {
          savedExperiences[index] = {
            id: editingExperienceId,
            name: currentExperienceName,
            sections: JSON.parse(JSON.stringify(sections))
          };
          lastSavedSections = JSON.stringify(sections);
          localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
          hidePopup("save-name-popup");
          closeAllPopups();
          renderAdmin();
          renderBusiness();
        }
      } else {
        // Prompt for name if unnamed
        closeAllPopups();
        document.getElementById("save-name-popup").style.display = "flex";
        document.getElementById("experience-name").value = "";
        document.getElementById("name-error").style.display = "none";
      }
    }

    /****************************
     * Save Changes Before Leaving
     ****************************/
    function saveChangesAndNavigate() {
      if (currentExperienceName && editingExperienceId !== null) {
        // Save to existing named experience
        const index = savedExperiences.findIndex(exp => exp.id === editingExperienceId);
        if (index !== -1) {
          savedExperiences[index] = {
            id: editingExperienceId,
            name: currentExperienceName,
            sections: JSON.parse(JSON.stringify(sections))
          };
          localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
          lastSavedSections = JSON.stringify(sections);
          hidePopup("save-changes-popup");
          closeAllPopups();
          resetToNewExperience();
          showPage(targetPage);
          renderAdmin();
        }
      } else {
        // Prompt for name for new experience
        hidePopup("save-changes-popup");
        closeAllPopups();
        document.getElementById("save-name-popup").style.display = "flex";
        document.getElementById("experience-name").value = "";
        document.getElementById("name-error").style.display = "none";
      }
    }

    function discardAndNavigate() {
      hidePopup("save-changes-popup");
      closeAllPopups();
      resetToNewExperience();
      showPage(targetPage);
    }

    /****************************
     * Duplicate Experience     *
     ****************************/
    function createDuplicatedExperience() {
      const name = document.getElementById("duplicate-experience-name").value.trim();
      if (!name) {
        document.getElementById("duplicate-name-error").textContent = "Please enter a name";
        document.getElementById("duplicate-name-error").style.display = "block";
        return;
      }
      if (savedExperiences.some(exp => exp.name === name)) {
        document.getElementById("duplicate-name-error").textContent = "Try again, this name is already in use";
        document.getElementById("duplicate-name-error").style.display = "block";
        return;
      }
      if (duplicatingExperienceIndex !== null) {
        const exp = savedExperiences[duplicatingExperienceIndex];
        const newExperience = {
          id: savedExperiences.length,
          name: name,
          sections: JSON.parse(JSON.stringify(exp.sections))
        };
        savedExperiences.push(newExperience);
        localStorage.setItem("savedExperiences", JSON.stringify(savedExperiences));
        duplicatingExperienceIndex = null;
        hidePopup("duplicate-experience-popup");
        renderAdmin();
      }
    }

    function hidePopup(popupId) {
      const popup = document.getElementById(popupId);
      if (popup) {
        popup.style.display = "none";
      }
      if (popupId === "save-name-popup") {
        document.getElementById("experience-name").value = "";
        document.getElementById("name-error").style.display = "none";
      } else if (popupId === "download-popup") {
        document.getElementById("download-email").value = "";
      } else if (popupId === "duplicate-experience-popup") {
        document.getElementById("duplicate-experience-name").value = "";
        document.getElementById("duplicate-name-error").style.display = "none";
        duplicatingExperienceIndex = null;
      } else if (popupId === "link-preview-popup") {
        document.getElementById("link-text").textContent = "";
      }
    }
    
    /***********************
     * Page Navigation     *
     ***********************/
    function showPage(page) {
      if (isClientView) {
        document.getElementById("client-page").style.display = "block";
        document.getElementById("navbar").style.display = "none";
        return;
      }

      if (currentPage === 'business' && page !== 'business' && hasUnsavedChanges()) {
        closeAllPopups();
        targetPage = page;
        document.getElementById("save-changes-popup").style.display = "flex";
        const saveYesBtn = document.getElementById("save-changes-yes-btn");
        if (!saveYesBtn.dataset.bound) {
          saveYesBtn.onclick = saveChangesAndNavigate;
          saveYesBtn.dataset.bound = true;
        }
        return;
      }

      // Update page visibility
      document.getElementById("business-page").style.display = (page === 'business') ? 'block' : 'none';
      document.getElementById("client-page").style.display = (page === 'client') ? 'block' : 'none';
      document.getElementById("admin-page").style.display = (page === 'admin') ? 'block' : 'none';
      
      // Update active tab styling
      const buttons = {
        'business': document.getElementById("nav-business"),
        'admin': document.getElementById("nav-admin")
      };
      Object.values(buttons).forEach(btn => btn.classList.remove('active'));
      if (buttons[page]) {
        buttons[page].classList.add('active');
      }

      // Reset to new experience when navigating to Experience Builder, unless editing
      if (page === 'business' && editingExperienceId === null) {
        resetToNewExperience();
      }

      currentPage = page;
      if (page === 'admin') {
        renderAdmin();
      } else if (page === 'client') {
        renderClient();
      } else {
        renderBusiness();
      }
    }
    
    /********************
     * Add New Section  *
     ********************/
    function addSection() {
      const newId = sections.length;
      sections.push({
        id: newId,
        name: "Name",
        images: []
      });
      renderBusiness();
    }

    /***********************
     * Initial Render      *
     ***********************/
    if (isClientView) {
      document.getElementById("navbar").style.display = "none";
      document.getElementById("business-page").style.display = "none";
      document.getElementById("admin-page").style.display = "none";
      document.getElementById("client-page").style.display = "block";
      renderClient();
    } else {
      renderBusiness();
      showPage('business'); // Start on Experience Builder
    }
  </script>
</body>
</html>
