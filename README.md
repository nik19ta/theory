# Принципы SOLID

- S => Single Responsibility Principle.

принцип единой ответственности говорит о том, что у каждого объекта есть своя ответственность и причина существования и эта ответственность у него только одна.

Например у нас есть класс `News` который отвечает только за новости и есть класс `NewsPrinter` который уже отвечает за вывод новостей будь то html/json/csv и тд

```js
class News {
    constructor(title, text) {
        this.title = title
        this.text = text
        this.modified = false
    }
    
    update(text) {
        this.text = text
        this.modified = true
    }
}

class NewsPrinter {
    constructor(news) {
        this.news = news
    }

    HTML() {
        return `
            <div class='news' >
                <h2>${this.news.title}</h2>
                <p>${this.news.HTMLtext}</p>
            </div>
        `
    }
    JSON() {
        return JSON.stringify({
            title: this.news.title,
            text: this.news.text,
            modified: this.news.modified
        }, null, 4)
    }
    CSV() {
        return `${this.news.title}/${this.news.text}/${this.news.modified}`
    }
}

const printer = new NewsPrinter(
    new News("Название", "Сам текст")
)

console.log(printer.HTML());
console.log(printer.JSON());
console.log(printer.CSV());
```
    
- O => Open Close Principle.
Принцип открытости/закрытости означает что програмные сущности (классы, модули, функции и т.д.) должны быть открыты для расширения и закрыты для изменений

Открыты для расширения. Это означает, что поведение модуля может быть расширено. То есть мы можем добавить модулю новое поведение в соответствии с изменившимися требованиями к приложению или для удовлетворения нужд новых приложений.

Закрыты для изменений. Исходный код такого модуля неприкасаем. Никто не вправе вносить в него изменения.

Например у нас есть две фигуры и у каждой фигуры есть метод который позволяет считать площадь и в данном примере если нам надо будет добавить премоугольник нам не надо будет менять класс `AreaCalc`

```js
class Shape {
    area() {
        throw new Error ("Area method should be implemented")
    }
}

class Square extends Shape {
    constructor(size) {
        super()
        this.size = size
    }

    area() {
        return this.size ** 2
    }
}

class Circle extends Shape {
    constructor(radius) {
        super()
        this.radius = radius
    }

    area() {
        return (this.radius ** 2) * Math.PI
    }
}

class AreaCalc {
    constructor(shapes) {
        this.shapes = shapes;    
    }

    sum() {
        return this.shapes.reduce((acc, shape) => {
            acc += shape.area()
            return acc
        }, 0)
    }
}


const calc = new AreaCalc([
    new Square(10),
    new Circle(1)
])

console.log(calc.sum());
```

- L => Liskov Substitution Principle.
- I => Interface Segregation Principle.
- D => Dependency Inversion Principle.