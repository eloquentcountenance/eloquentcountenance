from PIL import Image, ImageDraw
import cv2
import numpy as np

def cartoonize_image(input_path, output_path):
    img = cv2.imread(input_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    gray = cv2.medianBlur(gray, 5)
    edges = cv2.adaptiveThreshold(
        gray, 255,
        cv2.ADAPTIVE_THRESH_MEAN_C,
        cv2.THRESH_BINARY, 9, 9
    )
    color = cv2.bilateralFilter(img, 9, 300, 300)
    cartoon = cv2.bitwise_and(color, color, mask=edges)
    cv2.imwrite(output_path, cartoon)

def add_overlay(image_path, output_path):
    base = Image.open(image_path).convert("RGBA")
    overlay = Image.new('RGBA', base.size, (255, 255, 255, 0))
    draw = ImageDraw.Draw(overlay)
    # Draw a pink heart in the top-right corner
    heart_pos = (base.width - 100, 30)
    draw.ellipse([heart_pos, (heart_pos[0]+40, heart_pos[1]+40)], fill=(255, 105, 180, 255))
    # Composite images
    out = Image.alpha_composite(base, overlay)
    out.convert("RGB").save(output_path)

# Usage
cartoonize_image('input.jpg', 'cartoon_output.png')
add_overlay('cartoon_output.png', 'final_stylized_image.jpg')
<!--
**eloquentcountenance/eloquentcountenance** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
