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
    let difficulty = ""
    let answersObjects = []
    $: answersObjects = currentQuestion.incorrect_answers.map(answer => {
        return {
            answer, 
            right: false
        }
    })
    $: answersObjects = [...answersObjects, {answer: currentQuestion.correct_answer, right: true}]
    $: console.log(answersObjects)
    let correct = 0
    let incorrect = 0

    async function getQuestions(){
        fetch(`https://opentdb.com/api.php?amount=1&category=${category}&difficulty=${difficulty}&type=multiple`)
            .then(res => res.json())
            .then(data => {
                let result = data.results
                console.log(result)
                currentQuestion = result[0]
                console.log(currentQuestion)
                getAnswers()

            })
    }

    function checkAnswer(answer) {
        if(answer.right == true) {
            correct++
        } else {
            incorrect++
        }
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
<button on:click={getQuestions}>Generate Question</button>
</form>
<h2>{currentQuestion.question}</h2>
<form>
    <ul>
{#each answersObjects as answer}
        <li on:click={checkAnswer(answer)} on:keypress={checkAnswer(answer)}>{answer.answer}</li>
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