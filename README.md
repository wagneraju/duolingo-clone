<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clone Duolingo - Português para Inglês</title>
    <style>
        /* CSS será adicionado no Bloco 2 */
    </style>
</head>
<body>
    <div id="app">
        <!-- Cabeçalho com status do jogador -->
        <header>
            <h1>Duolingo Clone</h1>
            <div id="user-info">
                <span id="xp">XP: 0</span>
                <span id="level">Nível: 1</span>
                <span id="streak">🔥 0 dias</span>
            </div>
        </header>

        <main id="lesson-container">
            <!-- Tela inicial com lista de lições -->
            <div id="intro-screen">
                <h2>Aprenda Inglês</h2>
                <p>Escolha uma lição para começar:</p>
                <div id="lessons-list">
                    <!-- Lições serão geradas via JS -->
                </div>
            </div>

            <!-- Tela de exercícios -->
            <div id="exercise-screen" style="display: none;">
                <h2 id="exercise-question"></h2>
                <div id="exercise-options"></div>
                <input type="text" id="exercise-input" placeholder="Digite sua resposta" style="display:none;">
                <div id="order-words" style="display:none;"></div>
                <button id="submit-answer">Responder</button>
                <div id="feedback"></div>
            </div>

            <!-- Tela final da lição -->
            <div id="end-lesson" style="display:none;">
                <h2>🎉 Lição concluída!</h2>
                <p id="lesson-summary"></p>
                <button id="back-home">Voltar ao início</button>
            </div>
        </main>
    </div>

    <script>
        // JavaScript será adicionado no Bloco 3
    </script>
</body>
</html>
/* ===============================
   Estilo geral do aplicativo
================================= */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    background-color: #f6f7fb;
    color: #333;
}

header {
    background-color: #58cc02;
    padding: 15px;
    color: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header h1 {
    font-size: 1.5rem;
    margin: 0;
}

#user-info {
    display: flex;
    gap: 15px;
    font-weight: bold;
}

main {
    padding: 20px;
    max-width: 700px;
    margin: auto;
}

/* ===============================
   Tela inicial e lista de lições
================================= */
#lessons-list {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    margin-top: 20px;
}

.lesson-button {
    background-color: white;
    border: 2px solid #58cc02;
    padding: 15px;
    flex: 1 1 120px;
    border-radius: 15px;
    text-align: center;
    cursor: pointer;
    font-weight: bold;
    transition: 0.2s;
}

.lesson-button:hover {
    background-color: #58cc02;
    color: white;
}

/* ===============================
   Área de exercícios
================================= */
#exercise-screen {
    text-align: center;
}

#exercise-options {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: 15px 0;
}

.option-btn {
    background-color: white;
    border: 2px solid #ccc;
    padding: 10px;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.2s;
}

.option-btn:hover {
    background-color: #f0f0f0;
}

#exercise-input {
    padding: 10px;
    font-size: 1rem;
    border: 2px solid #ccc;
    border-radius: 10px;
    width: 80%;
    margin: auto;
}

/* ===============================
   Botão principal
================================= */
button {
    background-color: #58cc02;
    color: white;
    border: none;
    padding: 12px 20px;
    border-radius: 12px;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
    transition: 0.2s;
}

button:hover {
    background-color: #46a302;
}

/* ===============================
   Feedback visual
================================= */
.correct {
    background-color: #d4edda !important;
    border-color: #28a745 !important;
    color: #155724 !important;
}

.incorrect {
    background-color: #f8d7da !important;
    border-color: #dc3545 !important;
    color: #721c24 !important;
}

/* ===============================
   Tela de conclusão
================================= */
#end-lesson {
    text-align: center;
}

#lesson-summary {
    font-size: 1.2rem;
    margin-bottom: 20px;
}

/* ===============================
   Ordenar palavras
================================= */
.word-chip {
    display: inline-block;
    background-color: white;
    border: 1px solid #ccc;
    padding: 8px 12px;
    margin: 5px;
    border-radius: 12px;
    cursor: pointer;
}
(() => {
    // Banco de lições - 7 dias com 5 exercícios cada
    const lessons = [
        {
            day: 1,
            exercises: [
                // Tradução PT → EN
                {
                    type: 'translate_pt_en',
                    question: 'Olá',
                    answer: 'hello'
                },
                // Tradução EN → PT
                {
                    type: 'translate_en_pt',
                    question: 'Goodbye',
                    answer: 'adeus'
                },
                // Múltipla escolha
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "cachorro" em inglês?',
                    options: ['cat', 'dog', 'bird', 'fish'],
                    answer: 'dog'
                },
                // Completar lacuna
                {
                    type: 'fill_blank',
                    question: 'I ___ a student.',
                    answer: 'am'
                },
                // Ordenar palavras
                {
                    type: 'order_words',
                    question: ['I', 'am', 'a', 'teacher'],
                    answer: 'I am a teacher'
                }
            ]
        },
        {
            day: 2,
            exercises: [
                {
                    type: 'translate_pt_en',
                    question: 'Obrigado',
                    answer: 'thank you'
                },
                {
                    type: 'translate_en_pt',
                    question: 'Please',
                    answer: 'por favor'
                },
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "livro" em inglês?',
                    options: ['pen', 'book', 'table', 'chair'],
                    answer: 'book'
                },
                {
                    type: 'fill_blank',
                    question: 'She ___ my friend.',
                    answer: 'is'
                },
                {
                    type: 'order_words',
                    question: ['You', 'are', 'welcome'],
                    answer: 'You are welcome'
                }
            ]
        },
        {
            day: 3,
            exercises: [
                {
                    type: 'translate_pt_en',
                    question: 'Casa',
                    answer: 'house'
                },
                {
                    type: 'translate_en_pt',
                    question: 'Water',
                    answer: 'água'
                },
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "gato" em inglês?',
                    options: ['dog', 'cat', 'bird', 'fish'],
                    answer: 'cat'
                },
                {
                    type: 'fill_blank',
                    question: 'They ___ happy.',
                    answer: 'are'
                },
                {
                    type: 'order_words',
                    question: ['This', 'is', 'my', 'car'],
                    answer: 'This is my car'
                }
            ]
        },
        {
            day: 4,
            exercises: [
                {
                    type: 'translate_pt_en',
                    question: 'Amigo',
                    answer: 'friend'
                },
                {
                    type: 'translate_en_pt',
                    question: 'Family',
                    answer: 'família'
                },
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "escola" em inglês?',
                    options: ['school', 'church', 'hospital', 'store'],
                    answer: 'school'
                },
                {
                    type: 'fill_blank',
                    question: 'He ___ a doctor.',
                    answer: 'is'
                },
                {
                    type: 'order_words',
                    question: ['We', 'are', 'students'],
                    answer: 'We are students'
                }
            ]
        },
        {
            day: 5,
            exercises: [
                {
                    type: 'translate_pt_en',
                    question: 'Trabalho',
                    answer: 'work'
                },
                {
                    type: 'translate_en_pt',
                    question: 'Happy',
                    answer: 'feliz'
                },
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "comida" em inglês?',
                    options: ['food', 'water', 'drink', 'bread'],
                    answer: 'food'
                },
                {
                    type: 'fill_blank',
                    question: 'I ___ hungry.',
                    answer: 'am'
                },
                {
                    type: 'order_words',
                    question: ['They', 'are', 'friends'],
                    answer: 'They are friends'
                }
            ]
        },
        {
            day: 6,
            exercises: [
                {
                    type: 'translate_pt_en',
                    question: 'Tempo',
                    answer: 'time'
                },
                {
                    type: 'translate_en_pt',
                    question: 'Book',
                    answer: 'livro'
                },
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "carro" em inglês?',
                    options: ['car', 'bus', 'train', 'bike'],
                    answer: 'car'
                },
                {
                    type: 'fill_blank',
                    question: 'She ___ reading.',
                    answer: 'is'
                },
                {
                    type: 'order_words',
                    question: ['He', 'is', 'tall'],
                    answer: 'He is tall'
                }
            ]
        },
        {
            day: 7,
            exercises: [
                {
                    type: 'translate_pt_en',
                    question: 'Feliz',
                    answer: 'happy'
                },
                {
                    type: 'translate_en_pt',
                    question: 'Work',
                    answer: 'trabalho'
                },
                {
                    type: 'multiple_choice',
                    question: 'Como se diz "água" em inglês?',
                    options: ['food', 'water', 'drink', 'bread'],
                    answer: 'water'
                },
                {
                    type: 'fill_blank',
                    question: 'They ___ playing.',
                    answer: 'are'
                },
                {
                    type: 'order_words',
                    question: ['I', 'am', 'happy'],
                    answer: 'I am happy'
                }
            ]
        }
    ];

    // Variáveis de controle
    let currentLessonIndex = null;
    let currentExerciseIndex = 0;
    let xp = 0;
    let level = 1;
    let streak = 0;
    let lastStudyDate = null;

    // Elementos DOM
    const introScreen = document.getElementById('intro-screen');
    const lessonsList = document.getElementById('lessons-list');
    const exerciseScreen = document.getElementById('exercise-screen');
    const exerciseQuestion = document.getElementById('exercise-question');
    const exerciseOptions = document.getElementById('exercise-options');
    const exerciseInput = document.getElementById('exercise-input');
    const submitAnswerBtn = document.getElementById('submit-answer');
    const feedback = document.getElementById('feedback');
    const endLessonScreen = document.getElementById('end-lesson');
    const lessonSummary = document.getElementById('lesson-summary');
    const backHomeBtn = document.getElementById('back-home');
    const xpSpan = document.getElementById('xp');
    const levelSpan = document.getElementById('level');
    const streakSpan = document.getElementById('streak');
    const orderWordsContainer = document.getElementById('order-words');

    // Função para salvar progresso no localStorage
    function saveProgress() {
        const progress = {
            xp,
            level,
            streak,
            lastStudyDate,
            currentLessonIndex
        };
        localStorage.setItem('duoCloneProgress', JSON.stringify(progress));
    }

    // Função para carregar progresso do localStorage
    function loadProgress() {
        const progress = JSON.parse(localStorage.getItem('duoCloneProgress'));
        if (progress) {
            xp = progress.xp || 0;
            level = progress.level || 1;
            streak = progress.streak || 0;
            lastStudyDate = progress.lastStudyDate || null;
            currentLessonIndex = progress.currentLessonIndex !== undefined ? progress.currentLessonIndex : null;
        }
        updateUI();
    }

    // Atualizar UI de XP, nível e streak
    function updateUI() {
        xpSpan.textContent = `XP: ${xp}`;
        levelSpan.textContent = `Nível: ${level}`;
        streakSpan.textContent = `🔥 ${streak} dia${streak !== 1 ? 's' : ''}`;
    }

    // Criar lista de lições para o usuário escolher
    function createLessonsList() {
        lessonsList.innerHTML = '';
        lessons.forEach((lesson, index) => {
            const btn = document.createElement('button');
            btn.textContent = `Lição Dia ${lesson.day}`;
            btn.className = 'lesson-button';
            btn.disabled = (index > 0 && !isLessonCompleted(index - 1));
            btn.addEventListener('click', () => startLesson(index));
            lessonsList.appendChild(btn);
        });
    }

    // Verifica se a lição foi concluída (simplesmente se o índice da lição atual é maior que o selecionado)
    function isLessonCompleted(index) {
        if (currentLessonIndex === null) return false;
        return currentLessonIndex > index;
    }

    // Inicia uma lição selecionada
    function startLesson(index) {
        currentLessonIndex = index;
        currentExerciseIndex = 0;
        introScreen.style.display = 'none';
        endLessonScreen.style.display = 'none';
        exerciseScreen.style.display = 'block';
        feedback.textContent = '';
        orderWordsContainer.style.display = 'none';
        exerciseInput.style.display = 'none';
        submitAnswerBtn.disabled = false;
        renderExercise();
    }

    // Renderiza o exercício atual baseado no tipo
    function renderExercise() {
        const lesson = lessons[currentLessonIndex];
        const exercise = lesson.exercises[currentExerciseIndex];

        exerciseQuestion.textContent = '';
        exerciseOptions.innerHTML = '';
        feedback.textContent = '';
        exerciseInput.value = '';
        exerciseInput.style.display = 'none';
        orderWordsContainer.style.display = 'none';

        switch (exercise.type) {
            case 'translate_pt_en':
                exerciseQuestion.textContent = `Traduza para inglês: "${exercise.question}"`;
                exerciseInput.style.display = 'block';
                break;

            case 'translate_en_pt':
                exerciseQuestion.textContent = `Traduza para português: "${exercise.question}"`;
                exerciseInput.style.display = 'block';
                break;

            case 'multiple_choice':
                exerciseQuestion.textContent = exercise.question;
                exercise.options.forEach(opt => {
                    const btn = document.createElement('button');
                    btn.textContent = opt;
                    btn.className = 'option-btn';
                    btn.addEventListener('click', () => checkMultipleChoice(opt));
                    exerciseOptions.appendChild(btn);
                });
                submitAnswerBtn.style.display = 'none';
                break;

            case 'fill_blank':
                exerciseQuestion.textContent = `Complete a frase: "${exercise.question}"`;
                exerciseInput.style.display = 'block';
                break;

            case 'order_words':
                exerciseQuestion.textContent = 'Ordene as palavras para formar a frase correta:';
                orderWordsContainer.style.display = 'block';
                submitAnswerBtn.style.display = 'block';
                renderOrderWords(exercise.question);
                break;
        }
    }

    // Renderiza chips clicáveis para ordenar palavras
    function renderOrderWords(words) {
        orderWordsContainer.innerHTML = '';
        const shuffled = shuffleArray(words);
        shuffled.forEach(word => {
            const chip = document.createElement('span');
            chip.textContent = word;
            chip.className = 'word-chip';
            chip.addEventListener('click', () => {
                selectedOrder.push(word);
                chip.style.visibility = 'hidden';
                updateOrderWordsDisplay();
            });
            orderWordsContainer.appendChild(chip);
        });
        selectedOrder = [];
        updateOrderWordsDisplay();
    }

    // Atualiza a exibição das palavras selecionadas na ordem escolhida
    function updateOrderWordsDisplay() {
        let display = selectedOrder.join(' ');
        exerciseInput.value = display;
    }

    // Checa resposta para múltipla escolha
    function checkMultipleChoice(selected) {
        const exercise = lessons[currentLessonIndex].exercises[currentExerciseIndex];
        if (selected.toLowerCase() === exercise.answer.toLowerCase()) {
            handleCorrect();
        } else {
            handleIncorrect();
        }
    }

    // Checa resposta para input textual (tradução e fill_blank)
    function checkInputAnswer() {
        const exercise = lessons[currentLessonIndex].exercises[currentExerciseIndex];
        let userAnswer = exerciseInput.value.trim().toLowerCase();
        let correctAnswer = exercise.answer.toLowerCase();

        if (userAnswer === correctAnswer) {
            handleCorrect();
        } else {
            handleIncorrect();
        }
    }

    // Função shuffle para misturar array (usado em ordenar palavras)
    function shuffleArray(array) {
        let arr = array.slice();
        for (let i = arr.length -1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [arr[i], arr[j]] = [arr[j], arr[i]];
        }
        return arr;
    }

    // Handler de resposta correta
    function handleCorrect() {
        feedback.textContent = '✔️ Correto!';
        feedback.className = 'correct';
        submitAnswerBtn.disabled = true;
        addXP(10);

        setTimeout(() => {
            nextExercise();
        }, 1200);
    }

    // Handler de resposta incorreta
    function handleIncorrect() {
        feedback.textContent = '❌ Incorreto! Tente novamente.';
        feedback.className = 'incorrect';
    }

    // Avança para o próximo exercício ou finaliza lição
    function nextExercise() {
        currentExerciseIndex++;
        if (currentExerciseIndex >= lessons[currentLessonIndex].exercises.length) {
            completeLesson();
        } else {
            submitAnswerBtn.disabled = false;
            exerciseInput.value = '';
            exerciseInput.style.borderColor = '#ccc';
            exerciseOptions.innerHTML = '';
            renderExercise();
        }
    }

    // Completa a lição e volta para tela inicial
    function completeLesson() {
        exerciseScreen.style.display = 'none';
        endLessonScreen.style.display = 'block';

        lessonSummary.textContent = `Você concluiu a Lição Dia ${lessons[currentLessonIndex].day}! 🎉\nXP acumulado: ${xp}\nNível atual: ${level}\nStreak: ${streak} dia${streak !== 1 ? 's' : ''}`;

        // Atualiza progresso
        updateStreak();
        saveProgress();
        createLessonsList();
        updateUI();
    }

    // Atualiza streak considerando se estudou em dias consecutivos
    function updateStreak() {
        const today = new Date().toDateString();
        if (lastStudyDate === today) {
            // mesmo dia, nada muda
            return;
        }
        const yesterday = new Date(Date.now() - 86400000).toDateString();

        if (lastStudyDate === yesterday) {
            streak++;
        } else {
            streak = 1; // reseta streak
        }
        lastStudyDate = today;
    }

    // Adiciona XP e calcula nível
    function addXP(points) {
        xp += points;
        const newLevel = Math.floor(xp / 100) + 1;
        if (newLevel > level) {
            level = newLevel;
            alert(`Parabéns! Você subiu para o nível ${level} 🥳`);
        }
    }

    // Voltar para tela inicial
    backHomeBtn.addEventListener('click', () => {
        endLessonScreen.style.display = 'none';
        introScreen.style.display = 'block';
        feedback.textContent = '';
    });

    // Botão enviar resposta
    submitAnswerBtn.addEventListener('click', () => {
        const exercise = lessons[currentLessonIndex].exercises[currentExerciseIndex];
        if (exercise.type === 'multiple_choice') {
            // não usa submit (botão escondido)
            return;
        } else if (exercise.type === 'order_words') {
            const userAnswer = exerciseInput.value.trim().toLowerCase();
            if (userAnswer === exercise.answer.toLowerCase()) {
                handleCorrect();
            } else {
                handleIncorrect();
            }
        } else {
            checkInputAnswer();
        }
    });

    // Inicialização do app
    function init() {
        loadProgress();
        createLessonsList();
        introScreen.style.display = 'block';
        exerciseScreen.style.display = 'none';
        endLessonScreen.style.display = 'none';
    }

    // Variável para armazenar ordem selecionada no exercício de ordenar palavras
    let selectedOrder = [];

    init();
})();
