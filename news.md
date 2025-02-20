---
title: "Annoucements"
layout: page
logo: logo.jpg
feature_image: "https://images.unsplash.com/photo-1561404075-6151fbe7e8db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
feature_text: |
  ## News
---
<style>
/* Only target elements within the .gallery-container */
.gallery-container {
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 75vh;
    background-color: rgba(154, 152, 152, 0.87);
    position: relative;
}

.gallery-container .gallery {
    position: relative;
    width: 800px;
    height: 475px;  /*  Important: Keep this explicit height */
    overflow: hidden;
    border-radius: 10px;
}

.gallery-container .gallery img {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 1s ease-in-out;
    display: none; /* Hide by default */
}

.gallery-container .gallery img.active {
    opacity: 1;
    display: block; /* Show when active */
}

/* --- Caption Styling --- */
.gallery-container .caption-container {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);  /* Semi-transparent black */
    text-align: center;
    padding: 10px 0;
    z-index: 5;
    color: white;
    font-size: 20px;
    font-weight: bold;
    border-top: 1px solid rgba(255, 255, 255, 0.3);
    box-sizing: border-box; /* Include padding and border in width */
    display: none; /* Initially hidden */
}

.gallery-container .caption-container.active {
    display: block;
}

/* Arrow styling - NO CHANGES HERE */
.gallery-container .arrow {
    background: rgba(0, 0, 0, 0.5);
    color: white;
    border: none;
    padding: 10px 15px;
    cursor: pointer;
    font-size: 24px;
    border-radius: 5px;
    z-index: 10;
    margin: 0 20px;
    transition: box-shadow 0.3s ease-in-out, background-color 0.3s ease-in-out;
    outline: none;
}

.gallery-container .arrow:hover {
    box-shadow: 0 0 10px lightblue;
}

.gallery-container .arrow:active {
    background-color: blue;
    box-shadow: none;
}

.gallery-container .arrow:focus:not(:focus-visible) {
    outline: none;
}

/* Dot styling - NO CHANGES HERE*/
.dots-container {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-top: 10px;
    position: relative;
    z-index: 20;
}

.dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 2px solid rgba(62, 59, 59, 0.7);
    background: rgba(255, 255, 255, 0.58);
    transition: background 0.3s ease-in-out, transform 0.3s ease-in-out;
    cursor: pointer;
}

.dot.active {
    background: rgba(87, 82, 82, 0.67);
    transform: scale(1.2);
}
</style>

<div class="gallery-container">
    <button class="arrow" onclick="prevImage()">◀</button>
    <div class="gallery" id="gallery">
        <!-- Images will be loaded here by JavaScript -->
    </div>
    <button class="arrow" onclick="nextImage()">▶</button>
</div>
<div class="dots-container" id="dots-container"></div>

<script>
  let currentIndex = 0;
  let images = []; // Store image elements
  let captions = []; // Store caption elements
  let dotsContainer;
  let autoSlideInterval;
  let autoSlideDelay = 8000;
  let autoSlideTimeout;
  let galleryDiv;

    // Fetch and parse the CSV data
    fetch('/gallery_picture_info.csv')
        .then(response => response.text())
        .then(csvData => {
            const rows = csvData.trim().split('\n').slice(1); // Skip header row
            rows.forEach(row => {
                const [image_url, caption, link] = row.split('||').map(item => item.trim().replace(/^"|"$/g, '')); //remove quotes

                const img = document.createElement('img');
                img.src = image_url;
                img.alt = caption; // Use caption as alt text

                 // Create caption element
                const captionDiv = document.createElement('div');
                captionDiv.classList.add('caption-container');
                captionDiv.textContent = caption;


                // Wrap in link if provided
                if (link) {
                    const a = document.createElement('a');
                    a.href = link;
                    a.target = "_blank";
                    a.appendChild(img);
                    galleryDiv.appendChild(a); // Add the <a> to the gallery
                 } else {
                    galleryDiv.appendChild(img);
                }
                galleryDiv.appendChild(captionDiv);
                images.push(img);
                captions.push(captionDiv);
            });

            // Now that images are loaded, initialize
            dotsContainer = document.getElementById('dots-container');
            images.forEach((_, index) => {
                const dot = document.createElement('div');
                dot.classList.add('dot');
                dot.addEventListener('click', () => {
                    showImage(index);
                    resetAutoSlide();
                });
                dotsContainer.appendChild(dot);
            });
           showImage(0); // Show first image
           startAutoSlide();
        });

    galleryDiv = document.getElementById('gallery');


    function updateUI(index) {
        // Update images
        images.forEach(img => img.classList.remove('active'));
        images[index].classList.add('active');

        // Update captions
        captions.forEach(caption => caption.classList.remove('active'));
        captions[index].classList.add('active');

        // Update dots
        const dots = document.querySelectorAll('.dot'); // Get dots *inside* updateUI
        dots.forEach(dot => dot.classList.remove('active'));
        dots[index].classList.add('active');
    }

    function showImage(index) {
        updateUI(index)
        currentIndex = index;
    }

  function nextImage() {
      currentIndex = (currentIndex + 1) % images.length;
      showImage(currentIndex);
      resetAutoSlide();
  }

  function prevImage() {
      currentIndex = (currentIndex - 1 + images.length) % images.length;
      showImage(currentIndex);
      resetAutoSlide();
  }

  function startAutoSlide() {
      autoSlideInterval = setInterval(() => {
          currentIndex = (currentIndex + 1) % images.length;
          showImage(currentIndex);
      }, 5025);
  }

  function resetAutoSlide() {
      clearInterval(autoSlideInterval);
      clearTimeout(autoSlideTimeout);
      autoSlideTimeout = setTimeout(startAutoSlide, autoSlideDelay);
  }
</script>

### Previous Events
#### 2025 -
<br>