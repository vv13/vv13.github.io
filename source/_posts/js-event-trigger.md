---
title: js事件触发机制
date: 2017-10-17 23:27:11
tags: js
---
事件捕获

由网景最先提出，事件会从最外层开始发生，直到最具体的元素，也就是说假如父元素与子元素都绑定有点击事件，又互相重叠，那么先出发的会是父元素的事件，然后再传递到子元素。

事件冒泡

由微软提出，事件会从最内从的元素开始发生，再向外传播，正好与事件捕获相反。



这两个概念都是为了解决页面中事件流的发生顺序，w3c采取了折中的办法，制定了统一的标准：先捕获再冒泡。

---



addEventListen(event, function, useCapture)添加事件的第三个参数默认值为false，即默认使用事件冒泡，若为true则使用事件捕获的机制，以下为测试代码：

    container.addEventListener('click', () => console.log('container'), true)
    child.addEventListener('click', () => console.log('child'), true)
    // 点击child, 输出: container，child
    
    container.addEventListener('click', () => console.log('container'))
    child.addEventListener('click', () => console.log('child'))
    // 点击child, 输出: child，container

假若还是在两个div中，希望点击子元素时不触发父元素的点击事件，我们就需要用到阻止冒泡的方式：stopPropagation，改写child的方法：

    child.addEventListener('click', e => {
      console.log('child')
      e.stopPropagation()
    });

说起了stopPropagation，还有一种方式为preventDefault，它的作用不是用于阻止冒泡，而是阻止浏览器默认行为，如a标签跳转，submit提交等。

还有一种方式称为事件委托，利用冒泡的机制，子元素的点击事件可由父元素委托执行，举个例子，还是如上视图，子元素点击事件删除以后，对父元素做以下定义：

    container.addEventListener("click", e => {
      if (e.target.id === 'child') {
        console.log('child')
      }
    });

可见，当点击子元素依然会输出child，在某些特定场景利用事件委托可节省大量的性能。



明白了上述事件关系，target与currentTarget也就易于理解了，简言之，target指引发出发事件的元素，currentTarget则指事件绑定的元素，如通过点击子元素出发父元素，那么父元素中event对象的target为子元素，而currentTarget为它本身。
