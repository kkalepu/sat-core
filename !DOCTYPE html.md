<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>SAT Core - Daily Practice</title>  
    <script src="https://cdn.tailwindcss.com"></script>  
    <style>  
        .perspective-1000 { perspective: 1000px; }  
        .transform-style-3d { transform-style: preserve-3d; }  
        .backface-hidden { backface-visibility: hidden; }  
        .rotate-y-180 { transform: rotateY(180deg); }  
    </style>  
</head>  
<body class="bg-slate-900 text-slate-100 font-sans min-h-screen flex flex-col items-center justify-center p-4">  
      
    <header class="text-center mb-8">  
        <h1 class="text-3xl font-extrabold tracking-tight text-indigo-400">SAT CORE</h1>  
        <p class="text-sm text-slate-400 mt-1">Targeted Skill Conditioning</p>  
    </header>  
  
    <main class="w-full max-w-md flex flex-col items-center">  
        <div id="flashcard" class="w-full h-80 perspective-1000 cursor-pointer group">  
            <div id="card-inner" class="w-full h-full transform-style-3d transition-transform duration-500 relative bg-slate-800 rounded-2xl shadow-xl border border-slate-700">  
                  
                <div class="absolute inset-0 backface-hidden p-6 flex flex-col justify-between">  
                    <div>  
                        <span id="card-domain" class="text-xs font-bold uppercase tracking-wider text-indigo-400 bg-indigo-950/50 px-2.5 py-1 rounded-full border border-indigo-900">Domain</span>  
                        <p id="card-front-text" class="text-base font-medium mt-6 text-slate-200 leading-relaxed"></p>  
                    </div>  
                    <div class="text-center text-xs text-slate-500 font-medium">Tap card to reveal answer</div>  
                </div>  
                  
                <div class="absolute inset-0 backface-hidden rotate-y-180 p-6 flex flex-col justify-between bg-slate-800 rounded-2xl border border-slate-700">  
                    <div>  
                        <span class="text-xs font-bold uppercase tracking-wider text-emerald-400 bg-emerald-950/50 px-2.5 py-1 rounded-full border border-emerald-900">Strategy & Answer</span>  
                        <p id="card-back-text" class="text-sm mt-6 text-slate-300 whitespace-pre-line leading-relaxed"></p>  
                    </div>  
                    <div class="text-center text-xs text-indigo-400 font-semibold">Tap to flip back</div>  
                </div>  
  
            </div>  
        </div>  
  
        <div class="flex items-center justify-between w-full mt-8 px-2">  
            <button id="prev-btn" class="px-5 py-2.5 bg-slate-800 hover:bg-slate-700 active:scale-95 transition text-sm font-semibold rounded-xl border border-slate-700 text-slate-300 disabled:opacity-40">  
                Previous  
            </button>  
            <span id="card-indicator" class="text-sm font-medium text-slate-400"></span>  
            <button id="next-btn" class="px-5 py-2.5 bg-indigo-600 hover:bg-indigo-500 active:scale-95 transition text-sm font-semibold rounded-xl text-white shadow-lg disabled:opacity-40">  
                Next Card  
            </button>  
        </div>  
    </main>  
  
    <script>  
        const cards = [  
          {  
            "domain": "Reading: Information & Ideas",  
            "front": "A researcher claims that high urban population density does not inherently decrease community cohesion, provided that accessible public green spaces are readily available.\n\nWhat specific type of finding must textual or data evidence present to strongly support this hypothesis?",  
            "back": "The evidence must show a high-density urban area with abundant green spaces maintaining high or stable cohesion levels (matching or exceeding low-density spaces).\n\nStrategy: Beware of trap answers that show green spaces are highly utilized but fail to measure the community cohesion variable itself."  
          },  
          {  
            "domain": "Math: Algebra",  
            "front": "For a system of linear equations (two lines), what algebraic conditions must be true for the system to have exactly Zero Solutions?",  
            "back": "Slopes must be identical, but y-intercepts must be different (parallel lines that never intersect).\n\nStrategy: The SAT constantly tests this conceptually using constants (e.g., finding 'k' such that a system has no solution). Simply set the slopes equal to each other!"  
          },  
          {  
            "domain": "Reading: Information & Ideas",  
            "front": "When handling structural 'Inference' text questions, what is the safest rule of thumb to avoid picking a tempting high-level trap answer?",  
            "back": "The correct inference is always the most immediate, uncreative logical next step of the text's explicit premises.\n\nStrategy: If an answer choice requires you to assume an extra 'outside-the-text' step or uses extreme language (all, never, exclusively) that isn't explicitly supported, it is a trap."  
          },  
          {  
            "domain": "Math: Data Analysis",  
            "front": "If an extreme outlier value that is significantly larger than the rest of the data is added to a sample set, how will the mean and the median change?",  
            "back": "The Mean will increase significantly (it is sensitive to extreme values because it sums them all up).\n\nThe Median will change very little or stay completely unchanged (it only cares about middle positional ranking)."  
          },  
          {  
            "domain": "Math: Algebra",  
            "front": "When multiplying or dividing both sides of an inequality by a negative number to solve for x, what critical step must be performed to keep the statement mathematically true?",  
            "back": "You must flip the direction of the inequality sign (e.g., convert < to >).\n\nStrategy: This is the number one source of careless errors on grid-in or multiple-choice inequality questions. Do a quick mental check for negative operations."  
          }  
        ];  
  
        let currentIndex = 0;  
        let isFlipped = false;  
  
        const cardInner = document.getElementById('card-inner');  
        const frontText = document.getElementById('card-front-text');  
        const backText = document.getElementById('card-back-text');  
        const domainLabel = document.getElementById('card-domain');  
        const indicator = document.getElementById('card-indicator');  
        const prevBtn = document.getElementById('prev-btn');  
        const nextBtn = document.getElementById('next-btn');  
  
        function renderContent() {  
            const current = cards[currentIndex];  
            frontText.textContent = current.front;  
            backText.textContent = current.back;  
            domainLabel.textContent = current.domain;  
            indicator.textContent = `${currentIndex + 1} / ${cards.length}`;  
              
            prevBtn.disabled = currentIndex === 0;  
            nextBtn.disabled = currentIndex === cards.length - 1;  
        }  
  
        function updateCard() {  
            if (isFlipped) {  
                cardInner.classList.remove('rotate-y-180');  
                isFlipped = false;  
                setTimeout(renderContent, 250);  
            } else {  
                renderContent();  
            }  
        }  
  
        document.getElementById('flashcard').addEventListener('click', () => {  
            isFlipped = !isFlipped;  
            cardInner.classList.toggle('rotate-y-180', isFlipped);  
        });  
  
        prevBtn.addEventListener('click', (e) => {  
            e.stopPropagation();  
            if (currentIndex > 0) { currentIndex--; updateCard(); }  
        });  
  
        nextBtn.addEventListener('click', (e) => {  
            e.stopPropagation();  
            if (currentIndex < cards.length - 1) { currentIndex++; updateCard(); }  
        });  
  
        renderContent();  
    </script>  
</body>  
</html>  
