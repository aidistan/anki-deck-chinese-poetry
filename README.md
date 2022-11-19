# Anki Deck of Chinese Poetry (诗词)

## Aiming

This repository serves as a workspace for cotent collaboration. To download
the deck, please go to https://ankiweb.net/shared/info/629702782 instead.

## Template

For your infomation, following templates are used in the deck:

### Front Template

```html
<div id="title">《{{题目}}》— {{朝代}}·{{作者}}</div>
```

### Back Template

```html
{{FrontSide}}

<hr id="answer">

<div id="content">{{诗词}}</div>

<hr>

<div id="footnotes">
  <div>{{脚注}}</div>
  <div>{{诗注}}</div>
  <div>{{小传}}</div>
</div>

<script>
var content = document.getElementById('content')
var contents = content.innerHTML.split('<br>').map(function(line) {
  return '<div class="hidden">' + line + '</div>'
})
content.innerHTML = contents.join('')
var footnotes = document.getElementById('footnotes')
footnotes.classList = 'hidden'

var clock = {
  index: 0,
  timer: null,
  ticks: -1,
  click: false,
  tick: function () {
    if (this.timer) {
      clearTimeout(this.timer)
      this.timer = null
    }

    if (this.ticks > 35 || this.click === true) {
      content.children[this.index].classList = ''
      this.index += 1
      this.ticks = 0
      this.click = false
    } else {
      this.ticks += 1
    }

    if (this.index < contents.length) {
      this.timer = setTimeout(function () { clock.tick() }, 100)
    } else {
      footnotes.classList = ''
    }
  }
}

var handler = function () {
  if (clock.ticks > 5 && clock.ticks < 30) {
    clock.click = true
    clock.tick()
    clock.ticks = 5
  }
}

document.addEventListener('click', handler)
document.addEventListener('touchstart', handler)

clock.tick()
</script>
```

### Styling

```css
html {
  font-size: 16px;
  font-family: -apple-system, BlinkMacSystemFont, San Francisco, Segoe UI, Roboto, Helvetica Neue, sans-serif;
}

.hidden {
  visibility: hidden;
}

.card {
  color: black;
  background-color: white;
  text-align: center;
}

#title {
  font-size: 1.25rem;
  margin-bottom: 1em;
}

#content {
  padding: 1rem;
  cursor: pointer;
}

#content > div {
  min-height: 1em; /* for blank lines */
}

#footnotes {
  font-size: 1rem;
  text-align: left;
}

#footnotes > div {
  margin-bottom: 1em;
}

#footnotes ruby rt {
  font-size: 0.8rem;
}
```

## Acknowledgement

Thanks to following projects which make this deck possible:

- `chinese-poetry`: https://github.com/chinese-poetry/chinese-poetry
