<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI Resume Maker</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f4f6f9;
      padding: 30px;
      margin: 0;
    }
    .container {
      max-width: 900px;
      background: white;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      margin: auto;
      text-align: center;
    }
    h2 {
      font-size: 2.5em;
      margin-bottom: 20px;
      color: #333;
    }
    .form-group {
      margin: 15px 0;
    }
    input, textarea, button {
      width: 80%;
      padding: 12px;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 1em;
    }
    button {
      background-color: #007bff;
      color: white;
      cursor: pointer;
      font-weight: bold;
    }
    button:hover {
      background-color: #0056b3;
    }
    #profile-pic {
      width: 120px;
      height: 120px;
      border-radius: 50%;
      object-fit: cover;
      margin-bottom: 20px;
    }
    .resume-output {
      text-align: left;
      padding: 20px;
      background: #f9f9f9;
      border-radius: 8px;
      margin-top: 20px;
      font-family: 'Arial', sans-serif;
      font-size: 1.1em;
      color: #333;
      line-height: 1.6;
      white-space: pre-wrap;
    }
    .resume-header {
      display: flex;
      align-items: center;
      margin-bottom: 20px;
    }
    .resume-header img {
      margin-right: 20px;
    }
    .resume-header div {
      text-align: left;
    }
    .resume-header h3 {
      margin: 0;
      font-size: 2em;
    }
    .resume-header p {
      margin: 5px 0;
      font-size: 1.1em;
    }
    .skills, .experience, .social-links, .education, .hobbies {
      list-style-type: none;
      padding-left: 0;
    }
    .skills li, .experience li, .social-links li, .education li, .hobbies li {
      margin-bottom: 10px;
      font-size: 1.1em;
    }
    .social-links a {
      color: #007bff;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>AI Resume Maker</h2>
    
    <!-- Profile Photo Upload -->
    <input type="file" id="photo" accept="image/*" onchange="loadFile(event)" />
    <br />
    <img id="profile-pic" src="" alt="Profile Picture" />
    
    <!-- Personal Details Form -->
    <div class="form-group">
      <input type="text" id="name" placeholder="Full Name" />
    </div>
    <div class="form-group">
      <input type="text" id="email" placeholder="Email" />
    </div>
    <div class="form-group">
      <input type="text" id="phone" placeholder="Phone Number" />
    </div>
    <div class="form-group">
      <input type="text" id="address" placeholder="Address" />
    </div>
    <div class="form-group">
      <input type="text" id="linkedin" placeholder="LinkedIn URL" />
    </div>
    <div class="form-group">
      <input type="text" id="github" placeholder="GitHub URL" />
    </div>
    
    <!-- Resume Sections -->
    <div class="form-group">
      <textarea id="skills" rows="2" placeholder="Skills (comma-separated)"></textarea>
    </div>
    <div class="form-group">
      <textarea id="experience" rows="3" placeholder="Experience (short description)"></textarea>
    </div>
    <div class="form-group">
      <textarea id="education" rows="3" placeholder="Education (short description)"></textarea>
    </div>
    <div class="form-group">
      <textarea id="hobbies" rows="2" placeholder="Hobbies (comma-separated)"></textarea>
    </div>
    
    <!-- Resume Action Buttons -->
    <button onclick="generateResume()">Generate Resume</button>
    <button onclick="downloadPDF()">Download PDF</button>
    
    <!-- Resume Output -->
    <div id="resume" class="resume-output"></div>
  </div>

  <script>
    let photoData = '';

    function loadFile(event) {
      const output = document.getElementById('profile-pic');
      output.src = URL.createObjectURL(event.target.files[0]);
      photoData = output.src;
    }

    function generateResume() {
      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const phone = document.getElementById('phone').value;
      const address = document.getElementById('address').value;
      const linkedin = document.getElementById('linkedin').value;
      const github = document.getElementById('github').value;
      const skills = document.getElementById('skills').value;
      const experience = document.getElementById('experience').value;
      const education = document.getElementById('education').value;
      const hobbies = document.getElementById('hobbies').value;

      const summary = An enthusiastic professional with expertise in ${skills}. Proven experience in ${experience}. Passionate about continuous learning and growth.;

      const resumeHTML = `
        <div class='resume-header'>
          <img src='${photoData}' alt='Profile Picture' />
          <div>
            <h3>${name}</h3>
            <p>${email}</p>
            <p>${phone}</p>
            <p>${address}</p>
          </div>
        </div>
        <h4>Professional Summary</h4>
        <p>${summary}</p>
        <h4>Skills</h4>
        <ul class='skills'>
          ${skills.split(',').map(skill => <li>${skill.trim()}</li>).join('')}
        </ul>
        <h4>Experience</h4>
        <ul class='experience'>
          <li>${experience}</li>
        </ul>
        <h4>Education</h4>
        <ul class='education'>
          <li>${education}</li>
        </ul>
        <h4>Hobbies</h4>
        <ul class='hobbies'>
          ${hobbies.split(',').map(hobby => <li>${hobby.trim()}</li>).join('')}
        </ul>
        <h4>Social Links</h4>
        <ul class='social-links'>
          ${linkedin ? <li><a href='${linkedin}' target='_blank'>LinkedIn</a></li> : ''}
          ${github ? <li><a href='${github}' target='_blank'>GitHub</a></li> : ''}
        </ul>
      `;

      document.getElementById('resume').innerHTML = resumeHTML;
    }

    function downloadPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      const resumeContent = document.getElementById('resume').innerText;
      doc.text(resumeContent, 10, 10);
      doc.save('resume.pdf');
    }
  </script>
</body>
</html>
