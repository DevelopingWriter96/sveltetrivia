<script>
    import { element } from "svelte/internal";


    // let questionList = []
    // let questionIndex = 0
    let currentQuestion = {
        "category": "",
        "type": "multiple",
        "difficulty": "",
        "question": "",
        "correct_answer": "",
        "incorrect_answers": ["", "", ""]
    }
    let category = ""
    let difficulty =""
    let question = currentQuestion.question;
    let answers = currentQuestion.incorrect_answers
    let correct = 0
    let incorrect = 0
    async function getAnswers() {
        let correct = Math.floor(Math.random() * 4)
        console.log(correct)
        answers.splice(correct, 0, currentQuestion.correct_answer)
    }

    async function getQuestions(){
        fetch(`https://opentdb.com/api.php?amount=10&category=27&difficulty=easy`)
            .then(res => res.json())
            .then(data => {
                let result = data.results
                console.log(result)
            })
    }

    getAnswers()

    function checkAnswer(answer) {
        console.log(answer)
        // console.log(currentQuestion.correct_answer)
    }
</script>
<body>
<h1>Test your knowledge!</h1>
<h2>Select a Category and Difficulty</h2>
<form>
<select bind:value={category}>
    <option>Select a Category</option>
    <option value=9>General Knowledge</option>
    <option value=10>Entertainment: Books</option>
    <option value=11>Entertainment: Film</option>
    <option value=12>Entertainment: Music</option>
    <option value=13>Entertainment: Musicals and Theatres</option>
    <option value=14>Entertainment: Television</option>
    <option value=15>Entertainment: Video Games</option>
    <option value=16>Entertainment: Board Games</option>
    <option value=17>Science & Nature</option>
    <option value=18>Science: Computers</option>
    <option value=19>Science: Mathematics</option>
    <option value=20>Mythology</option>
    <option value=21>Sports</option>
    <option value=22>Geography</option>
    <option value=23>History</option>
    <option value=24>Politics</option>
    <option value=25>Art</option>
    <option value=26>Celebrities</option>
    <option value=27>Animals</option>
    <option value=28>Vehicles</option>
    <option value=29>Entertainment: Comics</option>
    <option value=30>Science: Gadgets</option>
    <option value=31>Entertainment: Japanese Anime & Manga</option>
    <option value=32>Entertainment: Cartoon and Animation</option>
</select>
<select bind:value={difficulty}>
    <option>Select a Difficulty</option>
    <option value="easy">Easy</option>
    <option value="medium">Medium</option>
    <option value="hard">Hard</option>
</select>
<button on:click={getQuestions}>Generate Questions</button>
</form>
<h2>{question}</h2>
<form>
    <ul>
{#each answers as answer}
        <li>{answer}</li>
{/each}
    </ul>
</form>
<p>Correct: {correct}</p>
<p>Incorrect: {incorrect}</p>
<div>

</div>
</body>
<style>
    * {
        background-color: #222;
        color: white;
    }

    ul li {
        list-style-type: none;
    }

    button {
        background-color: white;
        color: black;
    }
</style>