# Drum Kit Notes

### Overview
- when a user enters a letter on their keyboard, it will play a sound
- each `data-key` has it's own value and sound associated to it inside a <div> called keys
- when the key is hit (playing), it will call on the CSS to create the animation styling affect

### data-key and keyCode
```
const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
```

- `data-key` if you want to makeup something such as a `key`, you need to include it after `data`

- listen for the keydown event then use querySelector to select the audio associated with the data-key to play the sound

- the const key searches the the class=key styling to do the animations

- not entirely sure why but `` provided the noise and '' did not provide any noise

### Transition End
```
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
```

- for each key, after it's been played, perform the transition end or remove the animation effect

- removeTransition is a method we need to call on

### Styling

- in `.playing` describes what happens when the audio plays

- when the audio is playing, change the border color and scale up

- this playing class style is essentially adding the style when a certain key is hit

- need to remove the class to remove the styling after the audio is played; don't set a timeout because the time could be out of sync if someone wants to change it later on; use a transition end event

- click event (fire an event when someone clicked on me)

- transition event (I changed when something happens to me)

### Refactor

```
window.addEventListener('keydown', function(e) {
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if(!audio) return; //stop the function from running all together
    audio.currentTime = 0; //rewind to the start
    audio.play();
    key.classList.add('playing');
  })
```

- instead of calling hte function and defining it here, we will remove the code into it's own function like removeTransition

```
<script>
  function playSound(e) {
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if(!audio) return; //stop the function from running all together

    audio.currentTime = 0; //rewind to the start
    audio.play();
    key.classList.add('playing');
  }

  function removeTransition(e) {
    if (e.propertyName !== 'transform') return; //skip if its not a transform
    e.target.classList.remove('playing');
  }

  const keys = document.querySelectorAll('.key'); //select all they keys
  keys.forEach(key => key.addEventListener('transitionend', removeTransition));
  window.addEventListener('keydown', playSound);
</script>
```

- move this code into its own js file to create cleaner code and call that file in the script
