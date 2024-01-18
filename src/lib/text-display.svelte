<script lang="ts">
  import { randomNum } from './util'
  const origin = '문대승의 재밌는 코딩'.split('') 
  const randoms = Array.from({length: origin.length}, () => randomNum(10, 40))
  const specials = ['재', '밌' , '다']
  const exceptions = [' ', '!']
  

  let text = ''
  const intervalId = setInterval(() => {
    let newText = ''
    let completeCount = 0

    randoms.forEach((random, i) => {
      if(exceptions.includes(origin[i]) || random === 0) {
        newText += origin[i]
        if(random === 0) completeCount++
      } else {
        randoms[i]--
        newText += specials[random % specials.length]
      }
    })

    text = newText
    if(completeCount === origin.length) {
      clearInterval(intervalId)
      return
    }
  }, 100)

  

</script>

<div class="container">
  <p class="text">
    {text}
  </p>
</div>

<style>
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: calc(100vh - 24px);
  }

  .text {
    font-size: 32px;
  }

</style>