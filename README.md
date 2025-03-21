<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Image to PDF Converter</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="container">
    <h1>Image to PDF Converter</h1>
    <p>Upload your images and convert them to a single PDF file.</p>
    <input type="file" id="imageInput" accept="image/*" multiple>
    <button id="convertBtn">Convert to PDF</button>
    <div id="preview"></div>
  </div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="script.js"></script>
</body>
</html># Tic-Tac-Toe-gamebody {
  font-family: Arial, sans-serif;
  background-color: #f4f4f9;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  max-width: 500px;
  width: 100%;
}

h1 {
  color: #333;
}

p {
  color: #666;
}

input[type="file"] {
  margin: 20px 0;
  padding: 10px;
  border: 2px dashed #ccc;
  border-radius: 8px;
  width: 100%;
  cursor: pointer;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #0056b3;
}

#preview {
  margin-top: 20px;
}

#preview img {
  max-width: 100%;
  height: auto;
  margin: 10px 0;
  border-radius: 5px;
}const { jsPDF } = window.jspdf;

document.getElementById('convertBtn').addEventListener('click', convertToPDF);

function convertToPDF() {
  const images = document.querySelectorAll('#preview img');
  if (images.length === 0) {
    alert('Please upload at least one image.');
    return;
  }

  const pdf = new jsPDF('p', 'mm', 'a4');
  let yOffset = 10; // Vertical offset for images

  images.forEach((img, index) => {
    const imgWidth = pdf.internal.pageSize.getWidth() - 20; // Image width with margins
    const imgHeight = (img.naturalHeight * imgWidth) / img.naturalWidth; // Maintain aspect ratio

    if (index !== 0) {
      pdf.addPage(); // Add a new page for additional images
      yOffset = 10; // Reset Y offset for new page
    }

    pdf.addImage(img.src, 'JPEG', 10, yOffset, imgWidth, imgHeight);
    yOffset += imgHeight + 10; // Add spacing between images
  });

  pdf.save('converted.pdf');
}

document.getElementById('imageInput').addEventListener('change', function (event) {
  const preview = document.getElementById('preview');
  preview.innerHTML = ''; // Clear previous preview

  const files = event.target.files;
  for (let i = 0; i < files.length; i++) {
    const file = files[i];
    const reader = new FileReader();

    reader.onload = function (e) {
      const img = document.createElement('img');
      img.src = e.target.result;
      preview.appendChild(img);
    };

    reader.readAsDataURL(file);
  }
});
