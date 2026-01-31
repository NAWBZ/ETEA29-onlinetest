# ETEA29-onlinetest


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2)}
h2{text-align:center}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;border-radius:10px;border:2px solid #ddd}
button{padding:12px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;margin:5px}
#startBtn{background:#2980b9;color:#fff;width:100%}
#timer{background:#e74c3c;color:#fff;padding:15px;text-align:center;border-radius:10px;margin-bottom:10px}
.stats-bar{display:flex;justify-content:space-between;background:#f8f9fa;padding:10px;border-radius:10px;margin-bottom:15px;font-weight:600;font-size:14px;border:1px solid #eee}
.option{border:2px solid #ddd;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer}
.option.correct{background:#2980b9;color:#fff}
.option.wrong{background:#e74c3c;color:#fff}
.buttons{display:flex;justify-content:space-between}
#submitBtn{display:none;background:#27ae60;color:white;width:100%;margin-top:20px}
.subject-box{background:#f4f6f7;padding:12px;border-radius:10px;margin:8px 0}
.review-option.correct{background:#2980b9;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.review-option.wrong{background:#e74c3c;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.footer{text-align:center;color:#555;margin:20px 0;font-size:14px}
#resName{font-size: 22px; color: #2980b9; font-weight: 600;}
</style>
</head>
<body>

<div class="container center" id="loginDiv">
<h2>ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:28px;color:red"></div>
</div>

<div class="container" id="quizDiv" style="display:none">
<div id="timer">Time Left: 40:00</div>
<div class="stats-bar">
    <span id="qCounter">Question: 1/100</span>
    <span id="attemptedCounter">Attempted: 0/100</span>
</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button onclick="prevQ()">Previous</button>
<button onclick="skipQ()">Skip</button>
<button onclick="nextQ()" id="nextBtn" disabled>Next</button>
</div>
<button id="submitBtn" onclick="submitTest()">Submit Test</button>
</div>

<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resName"></p>
<p id="resScore"></p>
<p id="resTime"></p>
<h3>Subject-Wise Result</h3>
<div id="subjectResult"></div>
<button onclick="showReview()">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Test Review</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()">Back to Result</button>
</div>

<div class="footer">
Created by <b>MUHAMMAD FAIZAN NAWAB</b>
</div>

<script>
 const questions = [
   
    // Part 1: Cells and Biological Organization
    {subject:"Science", q:"What is defined as the basic structural and functional unit of all living things?", o:["Tissue","Organ","Cell","Organ system"], c:2},
    {subject:"Science", q:"Which of the following is the largest single cell in the world?", o:["Human nerve cell","Amoeba","Ostrich egg","Frog egg"], c:2},
    {subject:"Science", q:"Approximately how many cells make up the human body?", o:["7 billion","75 billion","750 billion","7.5 trillion"], c:3},
    {subject:"Science", q:"Which specific human cells have the ability to live throughout a person's entire life?", o:["Blood cells","Skin cells","Nerve cells","Intestinal cells"], c:2},
    {subject:"Science", q:"Which organelle is famously known as the 'Powerhouse of the Cell'?", o:["Nucleus","Ribosome","Mitochondria","Vacuole"], c:2},
    {subject:"Science", q:"What is the primary function of the cell's nucleus?", o:["To manufacture food","To control all cell activities","To store water","To protect the cell"], c:1},
    {subject:"Science", q:"The jelly-like substance that fills the cell and holds organelles is called:", o:["Nucleus","Cytoplasm","Cell wall","Vacuole"], c:1},
    {subject:"Science", q:"Which structure is found in plant cells but is completely absent in animal cells?", o:["Cell membrane","Nucleus","Cell wall","Mitochondria"], c:2},
    {subject:"Science", q:"In a plant cell, what is the function of the large central vacuole?", o:["Respiration","Protein synthesis","Storing food, water, and waste","Photosynthesis"], c:2},
    {subject:"Science", q:"Ribosomes are specialized organelles responsible for:", o:["Digestion","Respiration","Protein synthesis","Transport"], c:2},
    {subject:"Science", q:"Which cell part acts as a 'gatekeeper,' controlling substances entering or leaving?", o:["Nucleus","Cell wall","Cell membrane","Vacuole"], c:2},
    {subject:"Science", q:"What is the correct hierarchical order of biological organization?", o:["Cells → Organs → Tissues → Systems","Cells → Tissues → Organs → Organ systems","Tissues → Cells → Organs → Systems","Organ systems → Organs → Tissues → Cells"], c:1},
    {subject:"Science", q:"Which specialized cell is designed to carry messages as electrical signals?", o:["Muscle cell","Red blood cell","Nerve cell","Skin cell"], c:2},
    {subject:"Science", q:"The main function of a red blood cell is to:", o:["Fight germs","Carry oxygen","Produce hormones","Control body activities"], c:1},
    {subject:"Science", q:"Which organelle contains chlorophyll and helps plants perform photosynthesis?", o:["Mitochondria","Vacuole","Ribosome","Chloroplast"], c:3},
    {subject:"Science", q:"A group of similar cells working together to perform a specific task is a:", o:["Organ","Tissue","System","Organism"], c:1},
    {subject:"Science", q:"Organisms consisting of only a single cell (like Amoeba) are called:", o:["Multicellular","Unicellular","Non-cellular","Tissue-based"], c:1},
    {subject:"Science", q:"All organ systems in a human work in ___ to perform life processes.", o:["Isolation","Competition","Coordination","Opposition"], c:2},
    {subject:"Science", q:"Which organ system is responsible for transporting oxygen and nutrients?", o:["Digestive system","Muscular system","Circulatory system","Skeletal system"], c:2},
    {subject:"Science", q:"Which system works directly with the skeletal system to allow movement?", o:["Nervous system","Muscular system","Respiratory system","Excretory system"], c:1},

    // Part 2: Reproduction in Plants
    {subject:"Science", q:"Reproduction requiring a single parent producing identical offspring is:", o:["Sexual","Asexual","Pollination","Fertilization"], c:1},
    {subject:"Science", q:"The transfer of pollen from the anther to the stigma of a flower is:", o:["Fertilization","Germination","Pollination","Dispersal"], c:2},
    {subject:"Science", q:"Which part of the flower matures into a seed after fertilization?", o:["Ovary","Ovule","Stamen","Petal"], c:1},
    {subject:"Science", q:"In self-pollination, the pollen lands on the stigma of:", o:["A different species","The same flower or same plant","A different plant","The leaves"], c:1},
    {subject:"Science", q:"The female reproductive part (stigma, style, ovary) is called:", o:["Stamen","Carpel (Pistil)","Sepal","Anther"], c:1},
    {subject:"Science", q:"Growing a new plant from roots, stems, or leaves is called:", o:["Vegetative propagation","Seedling","Spore formation","Cross-pollination"], c:0},
    {subject:"Science", q:"Male gametes in a flowering plant are found inside the:", o:["Ovary","Pollen grains","Stigma","Style"], c:1},
    {subject:"Science", q:"Fertilization is specifically defined as the:", o:["Transfer of pollen","Growth of a seed","Fusion of male and female gametes","Falling of petals"], c:2},
    {subject:"Science", q:"Which of the following is an example of natural asexual reproduction?", o:["Grafting","Layering","Bulbs and Tubers","Cutting"], c:2},
    {subject:"Science", q:"Artificial propagation is used in agriculture primarily to:", o:["Slow growth","Decrease population","Lead to better quality yield","Stop reproduction"], c:2},
    {subject:"Science", q:"Methods such as Cutting, Grafting, and Layering are classified as:", o:["Natural","Artificial asexual reproduction","Sexual","Germination"], c:1},
    {subject:"Science", q:"A strawberry plant spreading through 'Runners' is an example of:", o:["Sexual reproduction","Artificial propagation","Natural asexual reproduction","Grafting"], c:2},

    // Part 3: Balanced Diet
    {subject:"Science", q:"A diet containing all essential nutrients in correct proportions is a:", o:["Vegetarian diet","Balanced diet","Liquid diet","High-protein diet"], c:1},
    {subject:"Science", q:"Which nutrient serves as the body’s primary source of immediate energy?", o:["Proteins","Vitamins","Carbohydrates","Minerals"], c:2},
    {subject:"Science", q:"Scurvy is a deficiency disease caused by a lack of:", o:["Vitamin A","Vitamin B","Vitamin C","Vitamin D"], c:2},
    {subject:"Science", q:"The primary role of proteins in the human body is:", o:["Providing energy","Growth and repair of tissues","Storing fat","Providing fiber"], c:1},
    {subject:"Science", q:"Which mineral is most important for the health of bones and teeth?", o:["Iron","Iodine","Calcium","Sodium"], c:2},
    {subject:"Science", q:"Night blindness is a condition resulting from a deficiency of:", o:["Vitamin D","Vitamin A","Iron","Vitamin C"], c:1},
    {subject:"Science", q:"Which component of food helps prevent constipation?", o:["Fats","Dietary Fiber (Roughage)","Proteins","Sugar"], c:1},
    {subject:"Science", q:"Anemia (weakness and pale skin) is caused by a deficiency of:", o:["Calcium","Sodium","Iron","Magnesium"], c:2},
    {subject:"Science", q:"According to your curriculum, which vitamins are highlighted as essential?", o:["B, E, and K","A, C, and D","B1 and B12","K and E"], c:1},
    {subject:"Science", q:"A balanced diet must include nutrients based on their ___ and food sources.", o:["Price","Chemical composition","Color","Weight"], c:1},
    {subject:"Science", q:"What is the relationship between a healthy diet and the body?", o:["No effect","Only affects weight","Correlates with fitness","Only for athletes"], c:2},
    {subject:"Science", q:"Which of the following is NOT a constituent of a balanced diet?", o:["Fats","Minerals","Carbon dioxide","Carbohydrates"], c:2},

    // Part 4: Digestive System
    {subject:"Science", q:"In humans, the process of digestion begins in the:", o:["Stomach","Mouth","Small intestine","Esophagus"], c:1},
    {subject:"Science", q:"What are the wave-like muscle contractions that move food?", o:["Circulation","Peristalsis","Absorption","Elimination"], c:1},
    {subject:"Science", q:"Which organ is responsible for producing bile to help digest fats?", o:["Pancreas","Liver","Gallbladder","Stomach"], c:1},
    {subject:"Science", q:"Most of the absorption of nutrients into the bloodstream occurs in the:", o:["Large intestine","Small intestine","Stomach","Mouth"], c:1},
    {subject:"Science", q:"Which acid is produced by the stomach to kill bacteria?", o:["Sulfuric acid","Acetic acid","Hydrochloric acid","Nitric acid"], c:2},
    {subject:"Science", q:"The primary function of the large intestine is to:", o:["Digest proteins","Absorb water from undigested waste","Produce saliva","Store bile"], c:1},


    // 51-80: ENGLISH GRAMMAR & VOCABULARY
    {subject:"English", q:"Identify the common noun in: 'The boy went to the park in Peshawar.'", o:["Peshawar","Boy","Went","To"], c:1},
    {subject:"English", q:"Which of the following is an abstract noun?", o:["Table","Happiness","Water","Teacher"], c:1},
    {subject:"English", q:"Choose the correct pronoun: 'Sara is my friend. __ is very kind.'", o:["He","It","She","They"], c:2},
    {subject:"English", q:"What is the adjective in: 'The hungry cat drank the milk.'", o:["Cat","Drank","Milk","Hungry"], c:3},
    {subject:"English", q:"Identify the verb: 'The students are playing in the ground.'", o:["Students","Playing","Ground","In"], c:1},
    {subject:"English", q:"Which word is an adverb? 'He ran quickly to catch the bus.'", o:["Ran","Quickly","Catch","Bus"], c:1},
    {subject:"English", q:"Choose the correct preposition: 'The book is __ the table.'", o:["In","On","Between","Over"], c:1},
    {subject:"English", q:"Select the conjunction: 'I like tea __ my sister likes coffee.'", o:["But","For","With","Under"], c:0},
    {subject:"English", q:"Choose the correct article: 'I saw __ apple on the tree.'", o:["A","An","The","No article"], c:1},
    {subject:"English", q:"Which sentence uses the correct article?", o:["He is a honest man.","He is an honest man.","He is honest man.","He is the honest man."], c:1},
    {subject:"English", q:"Choose the correct punctuation for: 'Where are you going'", o:[".","!","?",","], c:2},
    {subject:"English", q:"Identify the properly capitalized sentence:", o:["i live in pakistan.","I live in pakistan.","I live in Pakistan.","i Live in Pakistan."], c:2},
    {subject:"English", q:"Choose the correct form for Present Simple: 'She __ to school every day.'", o:["Go","Goes","Going","Gone"], c:1},
    {subject:"English", q:"Change to Past Simple: 'I eat an apple.'", o:["I eating an apple.","I ate an apple.","I will eat an apple.","I eats an apple."], c:1},
    {subject:"English", q:"Identify the Future Simple tense:", o:["We played cricket.","We are playing cricket.","We will play cricket.","We have played cricket."], c:2},
    {subject:"English", q:"Complete the sentence: 'They __ watching television now.'", o:["Is","Am","Are","Was"], c:2},
    {subject:"English", q:"What is the plural of 'Knife'?", o:["Knifes","Knive","Knives","Knifees"], c:2},
    {subject:"English", q:"Which of these is an irregular plural?", o:["Cats","Boys","Children","Apples"], c:2},
    {subject:"English", q:"What is the feminine gender of 'Lion'?", o:["Lioness","Lionessess","Female lion","Lions"], c:0},
    {subject:"English", q:"What is the masculine gender of 'Aunt'?", o:["Brother","Father","Uncle","Son"], c:2},
    {subject:"English", q:"Identify the Subject: 'The brave soldier fought the enemy.'", o:["Fought","The brave soldier","The enemy","Brave"], c:1},
    {subject:"English", q:"Identify the Object: 'The cat chased the mouse.'", o:["The cat","Chased","The mouse","Brave"], c:2},
    {subject:"English", q:"Choose the synonym of 'Big':", o:["Small","Large","Fast","Happy"], c:1},
    {subject:"English", q:"Choose the antonym of 'Hot':", o:["Warm","Cold","Sun","Bright"], c:1},
    {subject:"English", q:"Select the correctly spelled word:", o:["Receve","Receive","Recieve","Receeve"], c:1},
    {subject:"English", q:"Which type of sentence is this? 'Open the door.'", o:["Assertive","Interrogative","Imperative","Exclamatory"], c:2},
    {subject:"English", q:"Fill in with the correct degree: 'Aslam is __ than Ali.'", o:["Tall","Taller","Tallest","More tall"], c:1},
    {subject:"English", q:"Which is the 'Superlative' degree?", o:["Good","Better","Best","Most good"], c:2},
    {subject:"English", q:"Identify the silent letter in 'Walk':", o:["W","A","L","K"], c:2},
    {subject:"English", q:"Fill in the blank with the collective noun: 'A __ of bees.'", o:["Team","Swarm","Herd","Pack"], c:1},

    // 81-100: MATHEMATICS (INTEGERS, RATIOS, ALGEBRA)
    {subject:"Math", q:"Which of the following is a factor of every natural number?", o:["0","1","2","The number itself"], c:1},
    {subject:"Math", q:"A number that has only two factors (1 and itself) is called a:", o:["Composite number","Prime number","Even number","Odd number"], c:1},
    {subject:"Math", q:"What is the result of (-5) + (-8)?", o:["13","-13","3","-3"], c:1},
    {subject:"Math", q:"According to laws of multiplication, what is (-4) × (-3)?", o:["-12","12","7","-7"], c:1},
    {subject:"Math", q:"If the ratio of boys to girls is 3:2 and there are 12 girls, how many boys are there?", o:["8","18","15","24"], c:1},
    {subject:"Math", q:"A car travels 200 km in 4 hours. What is its rate of speed?", o:["800 km/h","50 km/h","40 km/h","0.02 km/h"], c:1},
    {subject:"Math", q:"What is 25% of 400?", o:["100","25","200","75"], c:0},
    {subject:"Math", q:"If A = {1, 2, 3} and B = {3, 4, 5}, what is A ∪ B?", o:["{3}","{1, 2, 3, 4, 5}","{1, 2, 4, 5}","{1, 2, 3, 3, 4, 5}"], c:1},
    {subject:"Math", q:"A set with no elements is called a/an:", o:["Infinite set","Empty (or Null) set","Singleton set","Universal set"], c:1},
    {subject:"Math", q:"In the algebraic expression 5x + 3, what is the 'coefficient'?", o:["x","5","3","+"], c:1},
    {subject:"Math", q:"Simplify the expression: 3a + 2b + 4a - b", o:["7a + b","7a + 3b","12ab","a + b"], c:0},
    {subject:"Math", q:"Solve the linear equation for y: y - 7 = 10", o:["3","17","-3","70"], c:1},
    {subject:"Math", q:"What is the value of x in the equation 3x = 15?", o:["12","45","5","18"], c:2},
    {subject:"Math", q:"The total area of all the faces of a 3D shape is called its:", o:["Volume","Surface area","Perimeter","Circference"], c:1},
    {subject:"Math", q:"A cube has a side length of 2 cm. What is its total surface area?", o:["8 cm²","24 cm²","12 cm²","16 cm²"], c:1},
    {subject:"Math", q:"Which property is shown by the equation a + b = b + a?", o:["Associative","Commutative","Distributive","Identity"], c:1},
    {subject:"Math", q:"What is the HCF (Highest Common Factor) of 12 and 18?", o:["36","2","6","3"], c:2},
    {subject:"Math", q:"Express 3/5 as a percentage:", o:["30%","60%","50%","35%"], c:1},
    {subject:"Math", q:"What is the result of |-15| (absolute value)?", o:["-15","0","15","1"], c:2},
    {subject:"Math", q:"In the ratio a : b, the first term 'a' is called the:", o:["Consequent","Antecedent","Proportion","Coefficient"], c:1},



];

 


let answers=new Array(questions.length).fill(null);
let index=0,time=2400,startTime,timerInt;

function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Fill all fields");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{
c--;countdown.innerText=c;
if(c===0){
clearInterval(i);
loginDiv.style.display="none";
quizDiv.style.display="block";
startTime=Date.now();
loadQ();startTimer();
}},1000);
}

function updateStats() {
    let attempted = answers.filter(a => a !== null).length;
    document.getElementById("qCounter").innerText = `Question: ${index + 1}/${questions.length}`;
    document.getElementById("attemptedCounter").innerText = `Attempted: ${attempted}/${questions.length}`;
}

function loadQ(){
let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. (${q.subject}) ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";
if(answers[index] === i) d.classList.add(i===q.c?"correct":"wrong");
d.innerText=op;
d.onclick=()=>{
if(answers[index]!=null)return;
answers[index]=i;
d.classList.add(i===q.c?"correct":"wrong");
nextBtn.disabled=false;
updateStats();
};
optionsBox.appendChild(d);
});
nextBtn.disabled=answers[index]==null;
submitBtn.style.display=index===questions.length-1?"block":"none";
updateStats();
}

function nextQ(){if(index<questions.length-1){index++;loadQ();}}
function prevQ(){if(index>0){index--;loadQ();}}
function skipQ(){index=(index+1)%questions.length;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";
resultDiv.style.display="block";
let score=0,subjectStats={};
questions.forEach((q,i)=>{
if(!subjectStats[q.subject]) subjectStats[q.subject]={total:0,correct:0};
subjectStats[q.subject].total++;
if(answers[i]===q.c){score++;subjectStats[q.subject].correct++;}
});
let pct=Math.round(score/questions.length*100);
resStatus.innerText=pct>=40?"PASSED":"FAILED";
resName.innerText=`Student Name: ${studentName.value}`;
resScore.innerText=`Overall Score: ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${Math.floor((Date.now()-startTime)/60000)} minutes`;
subjectResult.innerHTML="";
for(let s in subjectStats){
let x=subjectStats[s];
subjectResult.innerHTML+=`<div class="subject-box"><b>${s}</b>: ${x.correct}/${x.total} → ${Math.round(x.correct/x.total*100)}%</div>`;
}
}

function showReview(){
resultDiv.style.display="none";
reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let html=`<h4>${i+1}. ${q.q}</h4>`;
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c) cls+=" correct";
else if(answers[i]===idx) cls+=" wrong";
html+=`<div class="${cls}">${op}</div>`;
});
reviewContent.innerHTML+=html;
});
}

function backToResult(){
reviewDiv.style.display="none";
resultDiv.style.display="block";
}
</script>
</body>
</html>
