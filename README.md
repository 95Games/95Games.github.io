<!DOCTYPE html>
<html lang="en">
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
<style>
.container {
max-width: 800px;
margin-top: 50px;s
}
.img-container {
text-align: center;
margin-top: 20px;
}
.slider-container {
margin-top: 30px;
}
.compressed-image {
margin-top: 20px;
}
.preview-image {
max-width: 100%;
height: auto;
}
.image-info {
margin-top: 10px;
font-size: 16px;
}
.image-section {
margin-bottom: 40px;
}
 .upload-area {
      border: 2px dashed #ccc;
      padding: 20px;
      text-align: center;
      cursor: pointer;
    }
    .upload-area.dragover {
      background-color: #f1f1f1;
    }
</style>
</head>
<body>
<div class="container">
  <!-- Image upload section with drag and drop -->
    <div class="mb-3">
      <div id="uploadArea" class="upload-area">
        <label for="imageUpload" class="form-label">Choose an image to upload</label>
        <input type="file" class="form-control" id="imageUpload" accept="image/*" style="display: none;">
        <p>Drag and drop an image here</p>
      </div>
    </div>

<!-- Original Image and Size Display -->
<div class="image-section" id="originalImageSection" style="display:none;">
<h4>Original Image:</h4>
<div class="img-container" id="original-image-container">
<img id="original-image" class="preview-image" src="#" alt="Original Image">
</div>
<div class="image-info" id="original-size-info">
<strong>Original Size:</strong> <span id="originalSize"></span> KB
</div>
</div>

<!-- Slider for compression -->
<div class="slider-container">
<label for="compressionSlider" class="form-label">Compression Level</label>
<input type="range" class="form-range" id="compressionSlider" min="0" max="100" value="80">
<div class="d-flex justify-content-between">
<span>Low</span>
<span>High</span>
</div>
</div>

<!-- Button to compress image -->
<div class="text-center mt-4">
<button class="btn btn-primary" id="compressBtn">Compress Image</button>
</div>

<!-- Compressed Image and Size Display -->
<div class="image-section" id="compressedImageSection" style="display:none;">
<h4>Compressed Image:</h4>
<div class="img-container" id="compressed-image-container">
<img id="compressed-image" class="preview-image" src="#" alt="Compressed Image">
</div>
<div class="image-info" id="compressed-size-info">
<strong>Compressed Size:</strong> <span id="compressedSize"></span> KB
</div>
<!-- Download button for compressed image -->
<div class="text-center mt-4" id="download-container" style="display:none;">
<a id="downloadLink" href="#" class="btn btn-success" download="compressed-image.jpg">Download Compressed Image</a>
</div>
</div>
</div>

<!-- Include required libraries -->
<script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/compressorjs@1.0.7/dist/compressor.min.js"></script>
<script>
$(document).ready(function() {
let originalImageFile = null;
let compressedImageFile = null;
let originalImageSizeKB = 0;

// Handle image upload
$("#imageUpload").change(function(event) {
const file = event.target.files[0];
if (file) {
// Show original image and size
const reader = new FileReader();
reader.onload = function(e) {
// Display the original image
$("#original-image").attr("src", e.target.result);
$("#original-image-container").show();

// Display the original image size in KB
originalImageFile = file;
originalImageSizeKB = (file.size / 1024).toFixed(2); // Size in KB
$("#originalSize").text(originalImageSizeKB);
$("#originalImageSection").show();

// Hide compressed image section initially
$("#compressedImageSection").hide();
$("#download-container").hide();
};
reader.readAsDataURL(file);
}
});

// Compress image when button is clicked
$("#compressBtn").click(function() {
if (originalImageFile) {
// Get compression value from the slider
const compressionQuality = $("#compressionSlider").val();

// Create a new instance of Compressor.js and compress the image
new Compressor(originalImageFile, {
quality: compressionQuality / 100, // Set the quality level (between 0 and 1)
success(result) {
// Get the compressed image file and URL
compressedImageFile = result;
const compressedImageUrl = URL.createObjectURL(compressedImageFile);

// Calculate the compressed image size in KB
const compressedImageSizeKB = (compressedImageFile.size / 1024).toFixed(2); // Size in KB
$("#compressedSize").text(compressedImageSizeKB);
$("#compressed-size-info").show();

// Display the compressed image
$("#compressed-image").attr("src", compressedImageUrl);
$("#compressed-image-container").show();

// Show the download link for the compressed image
const downloadLink = $("#downloadLink");
downloadLink.attr("href", compressedImageUrl);
$("#download-container").show();

// Show compressed image section
$("#compressedImageSection").show();
},
error(err) {
console.error("Compression failed: ", err);
}
});
} else {
alert("Please upload an image first.");
}
});
});
</script>
</body>
</html>
