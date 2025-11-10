# goner
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Class Representative Voting</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      margin: 0;
      padding: 0;
    }

    h1 {
      text-align: center;
      margin-top: 20px;
    }

    .voter-info {
      text-align: center;
      font-size: 20px;
      margin-bottom: 20px;
    }

    .candidates {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
      margin: 20px;
    }

    .candidate {
      background: white;
      width: 250px;
      margin: 10px;
      padding: 15px;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      text-align: center;
    }

    .candidate img {
      width: 150px;
      height: 150px;
      object-fit: cover;
      border-radius: 50%;
      margin-bottom: 10px;
    }

    .candidate h3 {
      margin: 10px 0 5px;
    }

    .candidate p {
      font-size: 14px;
      color: #555;
    }

    .buttons {
      text-align: center;
      margin: 30px;
    }

    button {
      padding: 12px 25px;
      margin: 10px;
      font-size: 16px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
    }

    .vote-btn {
      background-color: #28a745;
      color: white;
    }

    .skip-btn {
      background-color: #dc3545;
      color: white;
    }

    #flashGreen, #flashRed {
      position: fixed;
      top: 0;
      left: 0;
      height: 100%;
      width: 100%;
      z-index: 9999;
      display: none;
    }

    #flashGreen {
      background-color: rgba(0, 255, 0, 0.6);
    }

    #flashRed {
      background-color: rgba(255, 0, 0, 0.5);
    }

    #thankyou {
      display: none;
      text-align: center;
      padding: 50px;
    }

    #thankyou h2 {
      font-size: 32px;
      margin-bottom: 20px;
    }

    #thankyou button {
      padding: 15px 30px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h1>Vote for Your Class Representative</h1>
  <div class="voter-info" id="voterNumber">Voter 1 of 75</div>

  <div class="candidates" id="votingSection">
    <div class="candidate">
      <img src="https://i.postimg.cc/5NnCKFsV/ADITYA-SMILE.jpg" alt="Candidate 1">
      <h3>Aditya Sharma</h3>
      <p>Environmental Engg | Leadership | Team Builder</p>
      <input type="radio" name="candidate" value="Aditya Sharma">
    </div>
    <div class="candidate">
      <img src="https://via.placeholder.com/150" alt="Candidate 2">
      <h3>Riya Patel</h3>
      <p>Creative Thinker | Cultural Leader | Friendly</p>
      <input type="radio" name="candidate" value="Riya Patel">
    </div>
    <div class="candidate">
      <img src="https://via.placeholder.com/150" alt="Candidate 3">
      <h3>Mohit Raj</h3>
      <p>Disciplined | Responsible | Sports Captain</p>
      <input type="radio" name="candidate" value="Mohit Raj">
    </div>
    <div class="candidate">
      <img src="https://via.placeholder.com/150" alt="Candidate 4">
      <h3>Diya Sharma</h3>
      <p>Class Topper | Kind-hearted | Excellent Speaker</p>
      <input type="radio" name="candidate" value="Diya Sharma">
    </div>
  </div>

  <div class="buttons">
    <button class="vote-btn" onclick="submitVote()">Vote</button>
    <button class="skip-btn" onclick="skipVote()">Skip</button>
  </div>

  <div id="flashGreen"></div>
  <div id="flashRed"></div>

  <div id="thankyou">
    <h2>🎉 Thank You for Voting!</h2>
    <button onclick="revote()">Revote</button>
    <button onclick="viewResults()">View Results</button>
  </div>

  <audio id="voteSound" src="https://www.soundjay.com/buttons/sounds/button-09.mp3" preload="auto"></audio>
  <audio id="skipSound" src="https://www.soundjay.com/buttons/sounds/button-10.mp3" preload="auto"></audio>

  <script>
    let currentVoter = 1;
    const totalVoters = 75;
    const results = {
      "Aditya Sharma": 0,
      "Riya Patel": 0,
      "Mohit Raj": 0,
      "Diya Sharma": 0,
      "Skipped": 0
    };

    function updateVoterDisplay() {
      if (currentVoter > totalVoters) {
        document.getElementById("votingSection").style.display = "none";
        document.querySelector(".buttons").style.display = "none";
        document.getElementById("voterNumber").style.display = "none";
        document.getElementById("thankyou").style.display = "block";
      } else {
        document.getElementById("voterNumber").innerText = `Voter ${currentVoter} of ${totalVoters}`;
        document.querySelectorAll('input[name="candidate"]').forEach(el => el.checked = false);
      }
    }

    function flashScreen(id) {
      const screen = document.getElementById(id);
      screen.style.display = "block";
      setTimeout(() => {
        screen.style.display = "none";
      }, 500);
    }

    function submitVote() {
      const radios = document.getElementsByName('candidate');
      let selected = null;

      for (let i = 0; i < radios.length; i++) {
        if (radios[i].checked) {
          selected = radios[i].value;
          break;
        }
      }

      if (selected) {
        results[selected]++;
        document.getElementById("voteSound").play();
        flashScreen("flashGreen");
        currentVoter++;
        updateVoterDisplay();
      } else {
        alert("⚠️ Please select a candidate before voting.");
      }
    }

    function skipVote() {
      results["Skipped"]++;
      document.getElementById("skipSound").play();
      flashScreen("flashRed");
      currentVoter++;
      updateVoterDisplay();
    }

    function revote() {
      location.reload();
    }

    function viewResults() {
      let resultText = "📊 Final Results:\n\n";
      for (const [key, value] of Object.entries(results)) {
        resultText += `${key}: ${value} votes\n`;
      }
      alert(resultText);
    }
  </script>
</body>
</html>
