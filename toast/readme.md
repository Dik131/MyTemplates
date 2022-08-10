Подключение и использование
Компонент для показа всплывающих уведомлений состоит из 2 файлов: «toast.css» и «toast.js». Преимуществом данной библиотеки состоит в том, что она имеет очень маленький размер («toast.min.js» немного больше 1Кбайта). В отличие от библиотеки jGrowl эти сообщения не требуют библиотеку jQuery, что для многих сайтов очень важно.

Подключение компонента осуществляется посредством:

включения на страницу стилей «toast.min.css»;
добавления JavaScript кода «toast.js».
<link href="path/to/toast.min.css" rel="stylesheet">
<script src="path/to/toast.min.js"></script>
Вывод всплывающего сообщения на страницу осуществляется посредством создания экземпляра объекта Toast:

/*
  title - название заголовка
  text - текст сообщения
  theme - тема
  autohide - нужно ли автоматически скрыть всплывающее сообщение через interval миллисекунд
  interval - количество миллисекунд через которые необходимо скрыть сообщение
*/
new Toast({
  title: 'Заголовок',
  text: 'Сообщение...',
  theme: 'light',
  autohide: true,
  interval: 10000
});
Если нужно создать сообщение без заголовка, то нужно просто ключу title установить значение false:

// без заголовка
new Toast({
  title: false,
  text: 'Сообщение...',
  theme: 'light',
  autohide: true,
  interval: 10000
});
Подробное описание
Создание HTML кода всплывающих сообщений как с заголовком, так и без него выполняется в JavaScript. Целью является создание следующей структуры:

<!-- без заголовка -->
<div class="toast toast_message toast_default">
  <div class="toast__body">Сообщение...</div>
  <button class="toast__close" type="button"></button>
</div>

<!-- с заголовком -->
<div class="toast toast_default">
  <div class="toast__header">Заголовок</div>
  <div class="toast__body">Сообщение...</div>
  <button class="toast__close" type="button"></button>
</div>
HTML код сообщений простой. Он состоит из элемента с классом toast, в котором в зависимости от типа уведомления расположены два или три элемента:

<div> с классом toast__header - заголовок;
<div> с классом toast__body - элемент, в котором выводится само сообщение;
<button> с классом toast__close - кнопка, для закрытия сообщения.
С помощью классов в CSS добавляются стили к этим элементам:

/* CSS-переменные */
:root {
  --toast-border-radius: 0.25rem;
  --toast-theme-default: #fff;
}

.toast {
  font-size: 0.875rem;
  background-clip: padding-box;
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: var(--toast-border-radius);
  box-shadow: 0 .125rem .25rem rgba(0, 0, 0, 0.075);
  display: none;
  position: relative;
  overflow: hidden;
}

.toast_default {
  color: #212529;
  background-color: var(--toast-theme-default);
}

.toast:not(:last-child) {
  margin-bottom: 0.75rem;
}

.toast__header {
  position: relative;
  padding: 0.5rem 2.25rem 0.5rem 1rem;
  background-color: rgba(0, 0, 0, 0.03);
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.toast__close {
  content: "";
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  width: 0.875em;
  height: 0.875em;
  background: transparent url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23000'%3e%3cpath d='M.293.293a1 1 0 011.414 0L8 6.586 14.293.293a1 1 0 111.414 1.414L9.414 8l6.293 6.293a1 1 0 01-1.414 1.414L8 9.414l-6.293 6.293a1 1 0 01-1.414-1.414L6.586 8 .293 1.707a1 1 0 010-1.414z'/%3e%3c/svg%3e") center/0.875em auto no-repeat;
  border: 0;
  opacity: 0.5;
  cursor: pointer;
  transition: opacity 0.1s ease-in-out;
}

.toast__close:hover {
  opacity: 1;
}

.toast__body {
  padding: 1rem;
}

.toast_message .toast__body {
  padding-right: 2.25rem;
}
Класс toast__close ещё используется в обработчике события click. При нажатию на эту кнопку выполняется закрытие сообщения.

this._el.addEventListener('click', (e) => {
  if (e.target.classList.contains('toast__close')) {
    // вызываем метод, скрывающий сообщение
    this._hide();
  }
});
После того как JavaScript добавляет HTML код всплывающего сообщения на страницу, оно не отображается, т.к. по умолчанию оно имеет display: none. Его показ осуществляется после того, как к нему добавляется класс toast_show.

.toast_show {
  display: block;
}
Скрытие сообщения выполняется путём удаления класса toast_show.

Задание темы осуществляется посредством добавления класса.

Например, тема primary устанавливается так:

<div class="toast toast_primary">...</div>
Помещение элементов .toast выполняется в контейнер .toast-container. Его создание тоже осуществляется с помощью JavaScript, но только в том случае, если его нет на странице.

if (!document.querySelector('.toast-container')) {
  const container = document.createElement('div');
  container.classList.add('toast-container');
  document.body.append(container);
}
Данный элемент по умолчанию располагается в правом верхнем углу страницы и имеет ширину 270 пикселей. Если нужно расположить в другом месте, то измените стили.

:root {
  --toast-width: 270px;
}

.toast-container {
  position: fixed;
  top: 15px;
  right: 15px;
  width: var(--toast-width);
}
Написан код JavaScript в виде класса и имеет следующую структуру:

class Toast {
  constructor(params) { ... }
  _show() { ... }
  _hide() { ... }
  _create() { ... }
}