# flashcards-app
flash card umg
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards App</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .flashcard { width: 300px; height: 200px; border: 2px solid #333; border-radius: 10px; display: flex; align-items: center; justify-content: center; margin: 20px auto; font-size: 20px; cursor: pointer; background-color: #f8f8f8; }
        .controls { margin-top: 20px; }
        input, select, button { margin: 5px; padding: 10px; font-size: 16px; }
    </style>
</head>
<body>
    <h1>Flashcards App</h1>
    <select id="topicSelect"></select>
    <div class="flashcard" id="flashcard">Haz clic para ver la respuesta</div>
    <div class="controls">
        <input type="text" id="question" placeholder="Pregunta">
        <input type="text" id="answer" placeholder="Respuesta">
        <input type="text" id="topic" placeholder="Tema">
        <button onclick="addFlashcard()">Agregar Flashcard</button>
        <button onclick="nextFlashcard()">Siguiente</button>
    </div>
    <script>
        let flashcards = JSON.parse(localStorage.getItem('flashcards')) || {};
        let currentTopic = Object.keys(flashcards)[0] || "";
        let currentIndex = 0;
        let showAnswer = false;

        function saveFlashcards() {
            localStorage.setItem('flashcards', JSON.stringify(flashcards));
        }

        function updateTopics() {
            let topicSelect = document.getElementById('topicSelect');
            topicSelect.innerHTML = '';
            Object.keys(flashcards).forEach(topic => {
                let option = document.createElement('option');
                option.value = topic;
                option.textContent = topic;
                topicSelect.appendChild(option);
            });
            topicSelect.value = currentTopic;
        }

        function addFlashcard() {
            let question = document.getElementById('question').value;
            let answer = document.getElementById('answer').value;
            let topic = document.getElementById('topic').value;
            if (question && answer && topic) {
                if (!flashcards[topic]) flashcards[topic] = [];
                flashcards[topic].push({ question, answer });
                saveFlashcards();
                updateTopics();
                document.getElementById('question').value = '';
                document.getElementById('answer').value = '';
                document.getElementById('topic').value = '';
                alert('Flashcard agregada!');
            }
        }

        document.getElementById('topicSelect').addEventListener('change', function() {
            currentTopic = this.value;
            currentIndex = 0;
            showAnswer = false;
            updateFlashcard();
        });

        function nextFlashcard() {
            if (!flashcards[currentTopic] || flashcards[currentTopic].length === 0) {
                alert('No hay flashcards en este tema');
                return;
            }
            currentIndex = Math.floor(Math.random() * flashcards[currentTopic].length);
            showAnswer = false;
            updateFlashcard();
        }

        function updateFlashcard() {
            if (flashcards[currentTopic] && flashcards[currentTopic].length > 0) {
                document.getElementById('flashcard').innerText = flashcards[currentTopic][currentIndex].question;
            } else {
                document.getElementById('flashcard').innerText = 'No hay flashcards en este tema';
            }
        }

        document.getElementById('flashcard').addEventListener('click', function() {
            if (!flashcards[currentTopic] || flashcards[currentTopic].length === 0) return;
            showAnswer = !showAnswer;
            this.innerText = showAnswer ? flashcards[currentTopic][currentIndex].answer : flashcards[currentTopic][currentIndex].question;
        });

        updateTopics();
        updateFlashcard();
    </script>
</body>
</html>
