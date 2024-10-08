

---

# YouTube Video Extractor and Player

A simple script to extract and play YouTube videos with a minimal interface.




https://github.com/user-attachments/assets/d0ce7688-b16a-43a8-a575-d4797b0aebdf



## Overview

Some websites are designed to prevent users from easily obtaining YouTube links. This script provides a clean and simple interface for extracting and playing YouTube videos directly on these sites. It also includes a player that gives you full control over the video playback. Additionally, it features a trigger button to open the video in [FreeTube](https://freetubeapp.io/), a privacy-friendly alternative to YouTube that is ad-free.

## Features

- **Minimal Interface:** Extract YouTube video links with ease.
- **Built-in Player:** Control video playback directly within the interface.
- **FreeTube Support:** Option to open videos in FreeTube for an ad-free experience.

## Installation

To use this script, you can add it to your preferred browser using the following extensions:

### Option 1: User JavaScript and CSS
This extension allows you to inject custom JavaScript and CSS into any website. It’s ideal for adding this script where you need it most.

- **Install the extension:** [User JavaScript and CSS](https://chromewebstore.google.com/detail/user-javascript-and-css/nbhcbdghjpllgmfilhnhkllmkecfmpld)
- **Add the script:** Copy the code from this repository and paste it into the extension’s script editor.

### Option 2: Tampermonkey
If you already have Tampermonkey installed, you can easily add this script to it.

- **Install Tampermonkey:** [Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
- **Add the script:** Create a new userscript in Tampermonkey, then copy and paste the code from this repository.

## How to Use

1. **Inject the Script:** Once the script is added to your preferred extension, visit the website where you want to extract the YouTube video.
2. **Activate the Script:** The minimal interface will appear, allowing you to extract the video link.
3. **Control Playback:** Use the built-in player to watch the video, or click the FreeTube button to open it in FreeTube for an enhanced viewing experience.



```
window.onload = function() {
    (function() {
        const playButton = document.createElement("button");
        const popupContainer = document.createElement("div");
        const popupContent = document.createElement("div");
        const buttonGroup = document.createElement("div");
        const closeButton = document.createElement("button");
        const freetubeButton = document.createElement("button");
        const youtubeIframe = document.createElement("iframe");
        const videoList = document.createElement("div");
        const minimizeButton = document.createElement("button");

        // Styles for the floating button
        playButton.textContent = "Play";
        playButton.style.position = "fixed";
        playButton.style.bottom = "20px";
        playButton.style.right = "20px";
        playButton.style.padding = "15px 20px";
        playButton.style.backgroundColor = "#ff4757";
        playButton.style.color = "white";
        playButton.style.border = "none";
        playButton.style.borderRadius = "50px";
        playButton.style.cursor = "pointer";
        playButton.style.zIndex = "1000";
        playButton.style.fontSize = "18px";
        playButton.style.display = "none";

        // Styles for the popup container
        popupContainer.style.position = "fixed";
        popupContainer.style.top = "0";
        popupContainer.style.left = "0";
        popupContainer.style.width = "100%";
        popupContainer.style.height = "100%";
        popupContainer.style.backgroundColor = "rgba(0, 0, 0, 0.7)";
        popupContainer.style.display = "none";
        popupContainer.style.justifyContent = "center";
        popupContainer.style.alignItems = "center";
        popupContainer.style.zIndex = "1001";

        // Styles for the popup content
        popupContent.style.position = "relative";
        popupContent.style.width = "80%";
        popupContent.style.maxWidth = "800px";
        popupContent.style.backgroundColor = "#fff";
        popupContent.style.borderRadius = "10px";
        popupContent.style.padding = "10px";

        // Styles for the iframe
        youtubeIframe.style.width = "100%";
        youtubeIframe.style.height = "450px";
        youtubeIframe.style.border = "none";
        youtubeIframe.style.borderRadius = "10px";

        // Styles for the button group
        buttonGroup.style.position = "absolute";
        buttonGroup.style.top = "-40px";
        buttonGroup.style.right = "-40px";
        buttonGroup.style.display = "flex";
        buttonGroup.style.gap = "10px";

        // Styles for the close button
        closeButton.textContent = "Close";
        closeButton.style.backgroundColor = "#ff4757";
        closeButton.style.color = "white";
        closeButton.style.border = "none";
        closeButton.style.borderRadius = "20%";
        closeButton.style.padding = "10px";
        closeButton.style.cursor = "pointer";
        closeButton.style.fontSize = "16px";
        closeButton.style.zIndex = "1002";

        // Styles for the Freetube button
        freetubeButton.textContent = "Open in Freetube";
        freetubeButton.style.backgroundColor = "#1e90ff";
        freetubeButton.style.color = "white";
        freetubeButton.style.border = "none";
        freetubeButton.style.borderRadius = "50px";
        freetubeButton.style.padding = "10px 20px";
        freetubeButton.style.cursor = "pointer";
        freetubeButton.style.fontSize = "14px";
        freetubeButton.style.zIndex = "1002";

        // Styles for the video list
        videoList.style.position = "fixed";
        videoList.style.bottom = "100px";
        videoList.style.right = "20px";
        videoList.style.backgroundColor = "#fff";
        videoList.style.border = "1px solid #ccc";
        videoList.style.borderRadius = "8px";
        videoList.style.padding = "10px";
        videoList.style.maxHeight = "300px";
        videoList.style.overflowY = "auto";
        videoList.style.zIndex = "1000";
        videoList.style.display = "none";

        // Styles for the minimize button
        minimizeButton.textContent = "−";
        minimizeButton.style.position = "fixed";
        minimizeButton.style.bottom = "70px";
        minimizeButton.style.right = "20px";
        minimizeButton.style.backgroundColor = "#ff4757";
        minimizeButton.style.color = "white";
        minimizeButton.style.border = "none";
        minimizeButton.style.borderRadius = "20%";
        minimizeButton.style.padding = "5px";
        minimizeButton.style.cursor = "pointer";
        minimizeButton.style.fontSize = "16px";
        minimizeButton.style.zIndex = "1000";
        minimizeButton.style.display = "none";

        // Append elements to the DOM
        document.body.appendChild(playButton);
        buttonGroup.appendChild(closeButton);
        buttonGroup.appendChild(freetubeButton);
        popupContent.appendChild(buttonGroup);
        popupContent.appendChild(youtubeIframe);
        popupContainer.appendChild(popupContent);
        document.body.appendChild(popupContainer);
        document.body.appendChild(videoList);
        document.body.appendChild(minimizeButton);

        // Regular expressions to find YouTube links and embedded videos
        const youtubeLinkRegex = /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/watch\?v=|youtu\.be\/)([^\s&]+)/g;
        const youtubeEmbedRegex = /(?:https?:\/\/)?(?:www\.)?youtube\.com\/embed\/([^\s&]+)/g;

        // Find all YouTube links and embeds on the page
        const links = [...document.querySelectorAll("a"), ...document.querySelectorAll("iframe")];
        let videoUrls = [];
        let currentVideoUrl = '';

        links.forEach(link => {
            let href = link.href || link.src;

            if (href) {
                let match = href.match(youtubeLinkRegex) || href.match(youtubeEmbedRegex);
                if (match) {
                    videoUrls.push(match[0]);
                }
            }
        });

        if (videoUrls.length > 0) {
            playButton.style.display = "block";
        }

        // Create video list if there are multiple videos
        if (videoUrls.length > 1) {
            videoList.style.display = "block";
            minimizeButton.style.display = "block";

            const header = document.createElement("div");
            header.innerText = `Videos found: ${videoUrls.length}`;
            header.style.marginBottom = "10px";
            videoList.appendChild(header);

            videoUrls.forEach((url, index) => {
                let videoId = url.match(youtubeLinkRegex) ? url.split("v=")[1] : url.split("embed/")[1];
                let videoLink = document.createElement("a");
                videoLink.href = "#";
                videoLink.innerText = `Link ${index + 1}: https://www.youtube.com/watch?v=${videoId}`;
                videoLink.style.display = "block";
                videoLink.style.marginBottom = "5px";
                videoLink.addEventListener("click", function () {
                    currentVideoUrl = `https://www.youtube.com/watch?v=${videoId}`;
                    youtubeIframe.src = `https://www.youtube.com/embed/${videoId}`;
                    popupContainer.style.display = "flex";
                });

                videoList.appendChild(videoLink);
            });
        }

        // Open popup when play button is clicked
        playButton.addEventListener("click", function () {
            if (videoUrls.length === 1) {
                let videoId = videoUrls[0].split("v=")[1] || videoUrls[0].split("embed/")[1];
                currentVideoUrl = `https://www.youtube.com/watch?v=${videoId}`;
                youtubeIframe.src = `https://www.youtube.com/embed/${videoId}`;
                popupContainer.style.display = "flex";
            } else {
                videoList.style.display = videoList.style.display === "block" ? "none" : "block";
            }
        });

        // Close popup when close button is clicked
        closeButton.addEventListener("click", function () {
            popupContainer.style.display = "none";
            youtubeIframe.src = "";
        });

        // Open video in Freetube when Freetube button is clicked
        freetubeButton.addEventListener("click", function () {
            if (currentVideoUrl) {
                const freetubeUrl = `freetube://${currentVideoUrl}`;
                window.location.href = freetubeUrl;
            }
        });

        // Minimize and unminimize the video list
        minimizeButton.addEventListener("click", function () {
            if (videoList.style.display === "block") {
                videoList.style.display = "none";
                minimizeButton.textContent = "+";
            } else {
                videoList.style.display = "block";
                minimizeButton.textContent = "−";
            }
        });
    })();
};

```

## Contributing

Feel free to fork this repository, make changes, and submit pull requests. Your contributions are welcome!

## License

This project is licensed under the MIT License.
