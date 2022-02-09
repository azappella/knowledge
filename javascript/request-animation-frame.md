# request animation frame

## promise-based example

```javascript
function nextFrame() {
    return new Promise(resolve => {
        requestAnimationFrame(resolve)
    })
}

function randomDelay(min, max) {
    const delay = Math.random() * (max - min) + min
    const startTime = performance.now()

    return nextFrame().then(() => {
        if (performance.now() - startTime < delay) {
            return nextFrame()
        }
    })
}

function simulateTyping(text,{ in: target, min = 10, max = 80, iterator = text[Symbol.iterator]() }) {
    const { value } = iterator.next()
    if (!value) {
        return Promise.resolve()
    }

    return randomDelay(min, max).then(() => {
        target.insertAdjacentText('beforeend', value)
        simulateTyping(text, { in: target, min, max, iterator })
    })
}

simulateTyping('hello world', {
    in: document.querySelector('.fake-input'),
}).then(() => {
    console.log('done')
})

```

### async/await example
```javascript
async function nextFrame() {
    return new Promise(resolve => {
        requestAnimationFrame(resolve)
    })
}

async function randomDelay(min, max) {
    const delay = Math.random() * (max - min) + min
    const startTime = performance.now()

    while (performance.now() - startTime < delay) {
        await nextFrame()
    }
}

async function simulateTyping(text, { in: target, min = 10, max = 80 }) {
    for (const character of text) {
        await randomDelay(min, max)
        target.insertAdjacentText('beforeend', character)
    }
}

simulateTyping('hello world', {
    in: document.querySelector('.fake-input'),
}).then(() => {
    console.log('done')
})

```

```javascript

function nextFrame() {
    return new Promise(resolve => {
        requestAnimationFrame(resolve)
    })
}

async function randomDelay(delay) {
    const startTime = performance.now()

    return nextFrame().then(() => {
        if (performance.now() - startTime < delay) {
            return nextFrame()
        }
    })
}

```

### references

- https://stackoverflow.com/questions/61017477/requestanimationframe-inside-a-promise
- https://html5rocks.com/en/tutorials/speed/rendering/
- https://ricaud.me/blog/post/2017/10/Async/await%2C-a-more-compelling-example
- https://gist.github.com/motss/e0ec90012a7c0ef06043e77a9cd9a61e