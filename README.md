# Блок Progress

Руководство по подключению и использованию блока Progress

## 1. Подключение

Вставьте HTML разметку блока `<div class="progress">...</div>` расположенную в файле [index.html](/index.html "перейти к файлу") в свой проект.
Подключите к проекту CSS [progress.css](/assets/css/progress.css "перейти к файлу") и JS [progress.js](/assets/js/progress.js "перейти к файлу") файлы.

> Для настройки цветов и длительности анимаций используются следующие CSS-переменные (подключеные в файле [style.css](/assets/css/style.css "перейти к файлу")), добавьте их в свой проект и при необходимости переопределите их:

```css
:root {
  --animation-duration: 0.3s;
  --progress-color-accent: #0000ff;
  --progress-color-grey: #f0f0f0;
  --progress-color-dark: #d6d6d6;
  --bg-color-white: #fff;
  --border-color-black: #000;
}
```

> Также в данном примере используюется [normalize.css](/assets/css/normalize.css "перейти к файлу") и следующие CSS-правила, для корректного отображения рекомендуется также добавить их в свой проект:

```css
* {
  box-sizing: border-box;
}

h1,
h2,
h3,
h4,
h5,
h6,
p,
a {
  margin: 0;
}
```

## 2. API блока Progress

Эксемпляры блока progress выводятся в общую область видимости в массиве объектов `progressInstances`:

```js
window.progressInstances = progressInstances;
```

Каждый экземпляр имеет следующие методы для взаимодействия с его состоянием:

> getValue() - возвращает текущее значение Value у дуги блока Progress

```js
window.progressInstances[index].getValue();
```

> setValue(value) - устанавливает значение для дуги в диапазоне 0-100 на значение value. Если value содержит символы кроме цифр - они удаляются, а итоговое число приводится к дисапазону 0-100 и при отклонении значения округляется до допустимого экстремума. Принимает целые значения.

```js
window.progressInstances[index].setValue(value);
```

> toggleAnimation(isAnimated) - включает анимацию при которой дуга постепенно заполняет frame и удаляется с конца, цикл длится пока не получит состояние false. isAnimated принимает булевое значение (пустая строка ''. 0 и пустой аргумент будет принят за false).

```js
window.progressInstances[index].toggleAnimation(isAnimated);
```

> toggleRotation(isRotated) - включает дополнительную анимацию, которая добавляет вращение самой дуги по часовой стрелке. isRotated принимает булевое значение (пустая строка ''. 0 и пустой аргумент будет принят за false), но данный метод работает только при включенной анимации (последнее состояние toggleAnimation было true)

```js
window.progressInstances[index].toggleRotation(isRotated);
```

> toggleHiding(isHidden) - скрывает элемент frame, а также отключает элементы управления Value, Animate, Rotate. isHidden принимает булевое значение (пустая строка ''. 0 и пустой аргумент будет принят за false).

```js
window.progressInstances[index].toggleHiding(isHidden);
```

> reset() - сбрасывает состояние блока reset к начальному: анимации отключены, значение дуги 0.

```js
window.progressInstances[index].reset();
```

Блок Progress имеет 4 элемента ручного управления и frame в виде круга для визуализации:

1. Value - Поле для ввода числового значения в диапазоне 0-100, регулирующее прогресс заполнения дуги в процентах
2. Animate - Переключатель отвечающией за анимацию заполнения дуги
3. Rotate - Переключатель (работает только при включенном Animate), который добавляет вращение по часовой стрелке для дуги Progress
4. Hide - Переключатель отвечающие за скрывание элемента frame в блоке Progress

![Пример блока Progress](/example.jpg)

> На этапе создание экземпляров можно задать начальное состояние блока progress использую описанные выше методы:

```js
progressElements.forEach((element, index) => {
  const progress = new Progress(element, index);
  progressInstances.push(progress);

  // Тут можно например задать начальное состояние дуги
  progress.setValue(30);
});
```

## 3. Кастомизация блока Progress

При желании вы можете настроить размер frame с отображением результатов:

В файле [progress.css](/assets/css/progress.css "перейти к файлу")
вы можете увеличить или уменьшить размеры `width` и `height` контейнера `.progress-circle`, а также у свойств `.progress-circle__bg` и `.progress-circle__arc` вы можете изменить ширину дуги `stroke-width`.

```css
.progress-circle {
  width: 120px;
  height: 120px;
}

.progress-circle__bg {
  stroke-width: 10;
}

.progress-circle__arc {
  stroke-width: 10;
}
```

> Диаметр окружности frame не должен превышать размеры viewbox у svg в элементе `progress-circle`

> `(r + stroke-width / 2) * 2 = размер viewbox`

> При этом центры элементов `circle` в svg `cx` и `cy` должны быть равны половине размера viewbox

> Соблюдая эти правила вы можете изменить размеры frame

## 4. Дополнительно

На странице вы можете отобразить неограниченное количество блоков Progress, элементы управления привязываются к каждому блоку Progress соответсвенно.

Приложение адаптируется под ориентацию экрана
