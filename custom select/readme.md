Для подключения компонента к странице достаточно к ней просто подключить CSS и JavaScript файл этого компонента или поместить эти коды в соответствующие свои файлы.

<!-- подключаем CSS селекта -->
<link rel="stylesheet" href="custom-select.css">

<!-- подключаем JS селекта -->
<script src="custom-select.js"></script>
Как использовать компонент CustomSelect
Компонент CustomSelect можно использовать на странице 2 способами.

Первый способ подразумевает непосредственную вставку HTML-кода селектора на страницу:

<div class="select" id="select-1">
  
  <button type="button" class="select__toggle" name="car" value="" data-select="toggle" data-index="-1">Выберите из списка</button>
  
  <div class="select__dropdown">
    <ul class="select__options">
      <li class="select__option" data-select="option" data-value="volkswagen" data-index="0">Volkswagen</li>
      <li class="select__option" data-select="option" data-value="ford" data-index="1">Ford</li>
      <li class="select__option" data-select="option" data-value="toyota" data-index="2">Toyota</li>
      <li class="select__option" data-select="option" data-value="nissan" data-index="3">Nissan</li>
    </ul>
  </div>
</div>
Элемент <button> представляет собой кнопку, предназначенную для открытия выпадающего списка. Её действие в JavaScript коде определяется с помощью атрибута data-select="toggle".

Также кнопка содержит пару «имя=значение» (где значение – это значение выбранной опции). Имя устанавливается атрибутом name, а значение – value. Поэтому если CustomSelect поместить в форму, то значение выбранной опции вместе с другими данными будут отправлены на сервер.

data-index содержит индекс выбранной опции. Если по умолчанию не должна быть установлена какая-то опция, то атрибуту value следует задать пустую строку, а data-index – значение -1.

В select__options находятся опции: <li> с классом select__option. data-select="option" определяет действие аналогично с кнопкой, data-value – значение опции, а data-index – её индекс (порядковый номер).

Если изначально какая-то опция должна быть активна, то к ней необходимо добавить класс select__item_selected. А также поместить в кнопку: в value – её значение, в data-index – её индекс, в содержимое – её контент.

<div class="select" id="select-1">
  <button type="button" class="select__toggle" name="car" value="ford" data-select="toggle" data-index="1">Выберите из списка</button>
  <div class="select__dropdown">
    <ul class="select__options">
      <li class="select__option" data-select="option" data-value="volkswagen" data-index="0">Volkswagen</li>
      <li class="select__option select__option_selected" data-select="option" data-value="ford" data-index="1">Ford</li>
      <li class="select__option" data-select="option" data-value="toyota" data-index="2">Toyota</li>
      <li class="select__option" data-select="option" data-value="nissan" data-index="3">Nissan</li>
    </ul>
  </div>
</div>
После создания необходимой HTML-структуры необходимо активировать корневой элемент (.select) как CustomSelect с помощью JavaScript.

Выполняется это следующим образом:

// #select-1 - селектор для выбора элемента, который необходимо инициализировать как CustomSelect
const select1 = new CustomSelect('#select-1');
Второй способ предполагает использование CustomSelect без необходимости непосредственной вставки HTML-структуры компонента на страницу. Здесь достаточно лишь поместить контейнер (пустой элемент) в HTML-документ.

<div id="select-2"></div>
Варианты и дефолтный текст (начальное значение) селекту необходимо передать при создании объекта в виде аргумента.

const select2 = new CustomSelect('#select-2', {
  name: 'car', // значение атрибута name у кнопки
  targetValue: 'ford', // значение по умолчанию
  options: [['volkswagen', 'Volkswagen'], ['ford', 'Ford'], ['toyota', 'Toyota'], ['nissan', 'Nissan']], // опции
});
Значение options - это массив массивов. Первый элемент массива – это значение опции, а второй – её текстовое представление.

Если не нужно чтобы CustomSelect имел какое-то значение по умолчанию, то установите ключу targetValue пустую строку или вообще его не указывайте:

const select2 = new CustomSelect('#select-2', {
  name: 'car', // значение атрибута name у кнопки
  options: [['volkswagen', 'Volkswagen'], ['ford', 'Ford'], ['toyota', 'Toyota'], ['nissan', 'Nissan']], // опции
});
После инициализации селекта, доступны следующие свойства и методы:

value – позволяет как получить выбранную опцию, так и установить её;
selectedIndex – индекс выбранного элемента (нумерация начинается с 0);
show() – показывает выпадающий список с опциями;
hide() – скрывает dropdown меню;
toggle() – переключает видимость выпадающего меню;
dispose() - удаляет обработчики событий, связанных с этим селектом.
Использование свойства value:

// установим в качестве выбранной опции элемент со значением toyota
select2.value = 'toyota';
// получим значение выбранной опции
console.log( select2.value ); // toyota
Кроме этого value позволяет также сбросить выбранную опцию. Для этого value нужно установить пустую строку или значение не соответствующее ни одной из опций:

// сбросим выбранную опцию
select2.value = '';
Использование свойства selectedIndex:

// установим в качестве выбранной опции элемент с индексом 2
select2.selectedIndex = 2;
// получим индекс выбранной опции
console.log( select2.selectedIndex );
Если ни один из элементов не выбран, то selectedIndex возвращает -1:

select2.value = '';
// получим индекс выбранного элемента
console.log( select2.selectedIndex ); // -1
Сбросить выбранный элемент можно не только посредством value, но также, если установить selectedIndex число -1 или индекс элемента, которого нет:

select2.selectedIndex = -1;
Если нам необходимо выполнить некоторые действия при выборе элемента отличного от текущего, то мы можем воспользоваться событием select.change, генерируемым в JavaScript коде:

document.querySelector('#select-2').addEventListener('select.change', (e) => {
  const btn = e.target.querySelector('.select__toggle');
  // выбранное значение
  console.log(`Выбранное значение: ${btn.value}`);
   // индекс выбранной опции
  console.log(`Индекс выбранной опции: ${btn.dataset.index}`);
  // выбранный текст опции
  const selected = e.target.querySelector('.select__option_selected');
  const text = selected ? selected.textContent : '';
  console.log(`Выбранный текст опции: ${text}`);
});
Кроме как использовать событие, это действие также можно выполнить с помощью метода onSelected при создании экземпляра объекта CustomSelect:

2 способ (через метод onSelect):

new CustomSelect('#select-2', {
  name: 'car',
  targetValue: 'ford',
  data: [['volkswagen', 'Volkswagen'], ['ford', 'Ford'], ['toyota', 'Toyota'], ['nissan', 'Nissan']],
  onSelected(select, option) {
    // выбранное значение
    console.log(`Выбранное значение: ${select.value}`);
    // индекс выбранной опции
    console.log(`Индекс выбранной опции: ${select.selectedIndex}`);
    // выбранный текст опции
    const text = option ? option.textContent : '';
    console.log(`Выбранный текст опции: ${text}`);
  }
});
Как устроен компонент Select
Компонент Select построен с использованием HTML, CSS и JavaScript.

HTML код компонента Select:

<div class="select">
  <button type="button" class="select__toggle" name="car" value="" data-select="toggle" data-index="-1">Выберите из списка</button>
  <div class="select__dropdown">
    <ul class="elect__options">
      <li class="elect__option" data-select="option" data-value="volkswagen" data-index="0">Volkswagen</li>
      <li class="elect__option elect__option_selected" data-select="option" data-value="ford" data-index="1">Ford</li>
      <li class="elect__option" data-select="option" data-value="toyota" data-index="2">Toyota</li>
    </ul>
  </div>
</div>
Элемент с классом select определяет этот компонент. В нём находится вся HTML-структура селекта. Тег <button> с классом select__toggle и атрибутами data-select="toggle", data-index предназначен для отображения выбранного значения и открытия при нажатии на него выдающего списка с опциями. Само выпадающее меню реализовано посредством элемента select__dropdown. Оно с помощью CSS настраивается так, чтобы оно было расположено под <button>. Список вариантов (.select__options) организован посредством маркированного списка. Выбранный элемент в нём отмечается посредством добавления к нему класса select__option_selected.

Кроме непосредственной вставки HTML кода на страницу, предоставим также возможность создавать его автоматически с помощью JavaScript. Таким образом, на страницу будет достаточно поместить пустой элемент и инициализировать его как CustomSelect. Как устроен JavaScript код приведём ниже.

Классы элементов будем использовать в CSS для добавления к ним стилей, а data атрибуты - в JavaScript.

Для обёртки установлено относительное позиционирование. Это необходимо для того, чтобы выпадающее меню можно было позиционировать относительно неё.

.select {
  position: relative;
  ...
}
Элемент с классом select__toggle стилизуем в виде кнопки (текущий вариант в ней будем выводить как её содержимое).

.select__toggle {
  display: flex;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 0.3125rem;
  cursor: pointer;
  align-items: center;
  width: 100%;
  font-size: 1rem;
  padding: 0.375rem 0.75rem;
  line-height: 1.4;
  user-select: none;
  font-size: 1rem;
  justify-content: space-between;
  font-style: italic;
}
Иконку к кнопке добавим через псевдоэлемент ::after:

.select__toggle::after {
  content: '';
  width: 0.75rem;
  height: 0.75rem;
  background-size: cover;
  background-image: url('data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" height="100" width="100"%3E%3Cpath d="M97.625 25.3l-4.813-4.89c-1.668-1.606-3.616-2.41-5.84-2.41-2.27 0-4.194.804-5.777 2.41L50 52.087 18.806 20.412C17.223 18.805 15.298 18 13.03 18c-2.225 0-4.172.804-5.84 2.41l-4.75 4.89C.813 26.95 0 28.927 0 31.23c0 2.346.814 4.301 2.439 5.865l41.784 42.428C45.764 81.174 47.689 82 50 82c2.268 0 4.215-.826 5.84-2.476l41.784-42.428c1.584-1.608 2.376-3.563 2.376-5.865 0-2.26-.792-4.236-2.375-5.932z"/%3E%3C/svg%3E');
}
По умолчанию dropdown меню не будет показываться. Включение его отображения будем осуществлять посредством добавления к нему класса select_show:

.select_show .select__dropdown {
  display: block;
}
При этом при показе dropdown меню иконку будем поворачивать на 180 градусов посредством CSS трансформации:

.select_show .select__toggle::after {
  transform: rotate(180deg);
}
CSS код для стилизации dropdown меню:

.select__dropdown {
  display: none;
  position: absolute;
  top: 2.5rem;
  left: 0;
  right: 0;
  border: 1px solid #ccc;
  max-height: 10rem;
  overflow-y: auto;
  border-radius: 0.3125rem;
}
.select__options {
  margin: 0;
  padding: 0;
  list-style: none;
}
.select__option {
  padding: 0.375rem 0.75rem;
}
Стилизация при наведении на пункт меню:

.select__option:hover {
  background-color: #f5f5f5;
  cursor: pointer;
  transition: 0.2s background-color ease-in-out;
}

Код написан с использованием класса:


class CustomSelect {
  ...
}
Свойства и методы, которые не нужно использовать вне класса, начинаются с нижнего подчеркивания:

class CustomSelect {
  constructor(target, params) {
    this._elRoot = typeof target === 'string' ? document.querySelector(target) : target;
    ...
  }
  ...
  _onClick(e) { ... }
  ...
}
Подчеркивание перед именем — это общепринятое соглашение для именования свойств и методов, которые используются только внутри класса, обращаться к ним вне его не нужно.

Структура JavaScript кода:

class CustomSelect {
  constructor(target, params) { }
  // обработчик события click
  _onClick(e) { }
  // сбрасывает состояние, генерирует событие 'select.change'
  _reset() { }
  // обновляет значения атрибутов в зависимости от выбранной опции, генерирует событие 'select.change'
  _update(option) { }
  // при изменении выбранной опции
  _changeValue(option) { }

  // включает отображение выпадающего списка
  show() { }
  // скрывает список с опциями
  hide() { }
  // переключает список с опциями
  toggle() { }
  // удаления слушателей события click селекта
  dispose() { }
  // возвращает значение выбранной опции
  get value() { }
  // позволяет установить опцию по значению
  set value(value) { }
  // возвращает индекс выбранной опции
  get selectedIndex() { }
  // позволяет выбрать опцию по её индексу
  set selectedIndex(index) { }
}
// функция для генерации HTML-кода селекта в зависимости от переданных аргументов
CustomSelect.template = params => { };
// для закрытия открытого селекта при клике вне его
document.addEventListener('click', (e) => {
  if (!e.target.closest('.select')) {
    document.querySelectorAll(SELECTOR_ACTIVE).forEach(select => {
      select.classList.remove(CLASS_NAME_ACTIVE);
    });
  }
});