<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import { fly, fade } from 'svelte/transition';
	import questions from "./mbtiQuestions"

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});

	let start = false
	
	let results = [ ];
	
	let result={
		e:0,
		i:0,
		s:0,
		n:0,
		f:0,
		t:0,
		j:0,
		p:0,
	};

	let resultString = ""
	function endMbti(){
		resultString = ""
		if(results.length>=questions.length){
			for (let i=0; i <= results.length; i++){
				if(results[i] in result){
					result[results[i]] += 1
				}
			}
		}
		result.e>result.i? resultString+="e": resultString+="i"
		result.s>result.n? resultString+="s":resultString+="n"
		result.f>result.t? resultString+= "f":resultString+="t"
		result.j>result.p? resultString+= "j":resultString+="p"
		resultString=resultString
	}
	function clear(){
		window.location.reload()
	}
	
</script>

{#if !start}
	<div class="result" style="font-size: 60px; height: 70px; " >여름방학🍦</div>
	<div class="result" style="margin:0;" >MBTI</div>
	<button class="button-54" on:click={()=>{start=true}}>시작하기</button>
{/if}

{#if start}
	{#each questions as { id, q, yes, no }}
		{#if results.length == id}
			<div>
				<progress value={$progress} ></progress>
			</div>
			<div id="big" in:fly="{{ y: 10, duration: 500 }}" out:fade>
				<div class="question">
				{#each q.replace(/\n/g, '<br/>').split('<br/>') as singleQuestion}
				<h2>{singleQuestion}</h2>
				{/each}
				</div>

				<label>
					<input type=radio bind:group={id} on:click={setTimeout(() => {
						results[id] = yes;
						result = result
						progress.set((id+1)*100/questions.length*0.01) 
						}, 500)}>
					그렇다 ✅
				</label>
			

				<label>
					<input type=radio bind:group={id} on:click={setTimeout(()=>{results[id] = no
					result = result
					progress.set((id+1)*100/questions.length*0.01)
					},500)}>
					아니다 ❌
				</label>
			</div>
		{/if}
	{/each}
	{#if results.length==12}
	<div in:fly="{{ y: 10, duration: 500 }}" out:fade>
		<div class="result" in:fly="{{ y: 10, duration: 500 }}"> {resultString}</div>
		<button class="button-54" on:click={endMbti}>결과보기</button>
		<button class="button-54" on:click={clear}>다시하기</button>
	</div>
	{/if}
{/if}

<style>
	@import url('https://fonts.googleapis.com/css2?family=Do+Hyeon&display=swap');
	div{
		font-family: 'Do Hyeon', sans-serif;
	}

	#big {
		box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
		text-align: center;
		width: 90%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin: 0 auto;
		border-radius: 10px;
		height: 300px;
		padding: 15px;
		}
	progress {
		-webkit-appearance: none;
		display: block;
		width: 90%;
		margin-right:auto ;
		margin-left: auto;
		margin-bottom: 30px;
		margin-top: 30px;
	}
		

::-webkit-progress-bar {
  background-color: #032b39;
  border-radius: 20px;
}

::-webkit-progress-value {
  background-color: #D5EDF6;
  border-radius: 20px;
}
	
.button-54 {
  background-color: transparent;
  border: 1px solid #266DB6;
  box-sizing: border-box;
  color: #00132C;
  font-family: "Avenir Next LT W01 Bold",sans-serif;
  font-size: 16px;
  font-weight: 700;
  line-height: 24px;
  padding: 16px 23px;
  position: relative;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-54:hover,
.button-54:active {
  outline: 0;
}

.button-54:hover {
  background-color: transparent;
  cursor: pointer;
}

.button-54:before {
  background-color: #D5EDF6;
  content: "";
  height: calc(100% + 3px);
  position: absolute;
  right: -7px;
  top: -9px;
  transition: background-color 300ms ease-in;
  width: 100%;
  z-index: -1;
}

.button-54:hover:before {
  background-color: #6DCFF6;
}

@media (min-width: 768px) {
  .button-54 {
    padding: 16px 32px;
  }
}
button{
	display: block;
	margin-left: auto;
	margin-right: auto;
	margin-top: 50px;
}
	
input[type=radio]{
	background-color: #ffff;
	-webkit-appearance: none;
	-moz-appearance: none;
	border: 1px solid rgb(216, 216, 216);
	margin: 0;
}

input[type=radio]:checked{
	background: #032b39;
}

.question{
	width: 90%;
	height: 200px;
	margin-left: auto;
	margin-right: auto;
	display: flex;
	flex-direction: column;
	justify-content: center;
	
}

.result{
	text-align: center;
	margin-top: 50px;
	height: 100px;
	font-size: 80px;
	font-weight: 800;
}

label{
	font-size: 20px;
}

h2{
	margin-bottom: 0;
	margin-top: 5px;
}

</style>