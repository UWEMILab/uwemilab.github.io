---
title: "Announcements"
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

/*  Use loading="lazy" for HUGE performance boost */
.gallery-container .gallery img {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 0.5s ease-in-out; /* Shorter transition */
    display: none; /* Hide by default */
    loading: lazy;  /*  <---  KEY CHANGE HERE */
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
        {% for item in site.data.gallery %}
            {% if item.link %}
                <a href="{{ item.link }}" target="_blank">
                    <img src="{{ item.image_url }}" alt="{{ item.caption }}" {% if forloop.first %}class="active"{% endif %} loading="lazy">  <!-- Add loading="lazy" here too -->
                </a>
            {% else %}
                <img src="{{ item.image_url }}" alt="{{ item.caption }}" {% if forloop.first %}class="active"{% endif %} loading="lazy">  <!-- Add loading="lazy" here -->
            {% endif %}
            <div class="caption-container {% if forloop.first %}active{% endif %}">
                {{ item.caption }}
            </div>
        {% endfor %}
    </div>
    <button class="arrow" onclick="nextImage()">▶</button>
</div>
 <div class="dots-container" id="dots-container">
    {% for item in site.data.gallery %}
      <div class="dot {% if forloop.first %}active{% endif %}" data-index="{{ forloop.index0 }}"></div>
    {% endfor %}
</div>
<script>
// ... (Your existing JavaScript - NO CHANGES NEEDED) ...
 let currentIndex = 0;
      let images = document.querySelectorAll('#gallery img'); // Select *existing* images
      let captions = document.querySelectorAll('#gallery .caption-container');
      let dotsContainer = document.getElementById('dots-container');
      let autoSlideInterval;
      let autoSlideDelay = 8000;
      let autoSlideTimeout;

      function updateUI(index) {
          images.forEach(img => img.classList.remove('active'));
          images[index].classList.add('active');

          captions.forEach(caption => caption.classList.remove('active'));
          captions[index].classList.add('active');

          const dots = document.querySelectorAll('.dot');
          dots.forEach(dot => dot.classList.remove('active'));
          dots[index].classList.add('active');
      }


      function showImage(index) {
         updateUI(index);
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


    //  Set up click handlers for dots *after* they're created by Jekyll.
    document.querySelectorAll('.dot').forEach((dot, index) => {
        dot.addEventListener('click', () => {
          showImage(index);
          resetAutoSlide();
        });
      });


      showImage(0); // Show the first image.
      startAutoSlide(); // Start the autoslide.
</script>

## Previous Events
#### 2025 -
To be updated.
<br>
