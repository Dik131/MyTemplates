Создание модального окна
Что такое модальное окно
Модальное окно – это элемент интерфейса, которой визуально представляет собой «всплывающее окно», отображающееся над остальной частью страницы.

При этом показ окна обычно сопровождают затемнением всей другой части страницы. Это действие позволяет визуально отделить его от остального содержимого страницы, а также показать, что в данный момент только оно одно является активным элементом. При этом контент, расположенный под ним, делают недоступным (т.е. пользователь не сможет с ним взаимодействовать пока он не закроет это окно).

Вызов модального окна можно привязать к различным событиям на странице, но в большинстве сценариев это осуществляют при нажатии на кнопку или ссылку.

Изображение модального окна:

Вид модального окна, созданного с помощью JavaScript

Оно состоит из заголовка (хедера), основной части и футера.

В заголовке обычно выводят название окна и элемент, с помощью которого его можно закрыть. В основной части распологают содержимое, а в футере кнопки для выполнения различных действий.

Загрузка и установка модального окна
Проект модального окна расположен на GitHub. Перейти к нему можно по этой ссылке.

Процесс установки модального окна на страницу выполняется посредством подключения к ней его CSS и JavaScript файлов, или добавления их содержимого в соответствующие свои файлы.

<!-- Подключение CSS файла -->
<link rel="stylesheet" href="modal.css">

<!-- Подключения JavaScript файла -->
<script src="modal.js"></script>
Как создать и вызвать модальное окно
Эта реализация модального окна не требует непосредственного размещения его HTML кода на странице. Это выполняется программно.

Таким образом, для того чтобы создать его достаточно просто вызвать функцию $modal:

var modal = $modal();
При создании окна вы можете сразу же его настроить, для этого в данную функцию необходимо передать данные в формате объекта. Осуществляется это с помощью соответствующих ключей (свойств). Например, с помощью ключа title вы можете задать заголовок, который будет иметь всплывающее окно по умолчанию. Ключ content позволяет установить содержимое, а footerButtons – кнопки для отображения их в его нижней части (футере).

var modal = $modal({
  title: 'Текст заголовка',
  content: '<p>Содержимое модального окна...</p>',
  footerButtons: [
    { class: 'btn btn__cancel', text: 'Отмена', handler: 'modalHandlerCancel' },
    { class: 'btn btn__ok', text: 'ОК', handler: 'modalHandlerOk' }
  ]
});
Все эти ключи являются не обязательными. Если не указать title, то заголовок будет иметь название «Новое окно». Если не установить значению ключу content, то модальное окно в этом случае создатся с пустым содержимым.

Ключ footerButtons в отличие от title и content принимает в качестве значения массив объектов. Каждый объект в этом массиве представляет собой кнопку. Она задаётся с помощью ключей text, class, handler. С помощью них вы можете установить кнопке (элементу <button>) текст, значение атрибутов class и data-handler. Если ключ footerButtons вообще не указать, то в этом случае модальное окно будет создано без футера.

Пример кода, выполняющего создание модального окна без кнопок в нижней части, с заголовком «Новое сообщение» и пустым содержимым:

var modal = $modal({
  title: 'Текст заголовка'
});
Пример создания модального окна с настройками по умолчанию:

var modal = $modal();
Этот код создаст модальное окно без футера, с пустым содержимым и заголовком «Новое окно».

Но функция $modal не только создаёт модальное окно в DOM, но также предоставляет методы для управления им.

Для этого нужно создать переменную и присвоить ей результат выполнения функции $modal.

В эту созданную переменную будет помещён объект (а точнее ссылка на него), имеющий следующие методы:

show – для отображения модального окна;
hide – для скрытия модального окна;
destroy – для удаления модального окна из DOM и связанных с ним обработчиков событий;
setContent – для установки контента;
setTitle – для установки заголовка.
Эти методы предназначены для взаимодействия с созданным окном. Они позволяют его открыть, закрыть, изменить ему контент и др.

Рассмотрим, как работать с этими методами на примерах.

Например, метод show используется когда вам необходимо показать (открыть) модальное окно:

modal.show();
Метод hide применяется для его скрытия:

modal.hide();
Методы setContent и setTitle предназначены соответственно для изменения контента и заголовка модального окна после его создания.

modal.setContent('<p>Новое содержимое...</p>');
modal.setTitle('Текст нового заголовка');
В возвращаемом объекте также есть метод destroy. Его необходимо использовать только когда вам необходимо полностью удалить модальное окно из DOM, а также связанные с ним события:

modal.destroy();
Данную операцию имеет смысл использовать только в том случае, если созданное модальное окно вам больше не нужно на странице.

Примеры использования скрипта для создания модальных окон
1. Пример кода, выполняющий открытие модального окна при нажатии на определённую кнопку.

<button id="show-modal" class="btn">Открыть</button>
<script>
  // создаём модальное окно
  var modal = $modal();
  // при клике по кнопке #show-modal
  document.querySelector('#show-modal').addEventListener('click', function(e) {
    // отобразим модальное окно
    modal.show();
  });
</script>
2. Пример кода, позволяющий открыть одно и тоже модальное окно посредством клика на разные элементы (определяется элемент, который может открыть это окно, с помощью наличия у него атрибута data-toggle="modal"):

<button class="btn" data-toggle="modal">Кнопка 1</button>
<button class="btn" data-toggle="modal">Кнопка 2</button>
<script>
  // создаём модальное окно
  var modal = $modal();
  // при клике на документ
  document.addEventListener('click', function(e) {
    if (e.target.dataset.toggle === 'modal') {
      modal.show();
    }
  });
</script>
3. Пример, в котором заголовок и содержимое модального окна определяется посредством значений data-атрибутов элемента, с помощью которого оно вызывается:

<button class="btn" data-toggle="modal" data-title="Заголовок 1" data-content="Содержимое модального окна 1...">Кнопка 1</button>
<button class="btn" data-toggle="modal" data-title="Заголовок 2" data-content="Содержимое модального окна 2...">Кнопка 2</button>
<script>
// создаём модальное окно
var modal = $modal();
// при клике на документ
document.addEventListener('click', function (e) {
  if (e.target.dataset.toggle === 'modal') {
    modal.setTitle(e.target.dataset.title);
    modal.setContent(e.target.dataset.content);          
    modal.show();
  }
});
</script>
4. В этом примере показано как можно в обработчике события «click» для кнопки, расположенной в футере модального окна, получить элемент, посредством которого оно было открыто:

<div class="img__items">
  <div class="img__item">
    <img src="/examples/images/car-1.jpg" alt="" data-price="22500" data-name="Audi A5 Coupé">
  </div>
  ...
</div>

<script>
(function () {
  var elemTarget;
  // создаём модальное окно
  var modal = $modal({
    title: 'Просмотр изображения',
    content: '<img src="" alt="" style="display: block; height: auto; max-width: 100%;">',
    footerButtons: [
      { class: 'btn btn__delete', text: 'Удалить', handler: 'modalHandlerDelete' },
      { class: 'btn btn__cancel', text: 'Закрыть', handler: 'modalHandlerCancel' }
    ]
  });
  // при клике на документ
  document.addEventListener('click', function (e) {
    // если мы кликнули на измобржение расположенное в .img__items, то...
    if (e.target.matches('.img__items img')) {
      elemTarget = e.target;
      // устанавливаем модальному окну title
      modal.setContent('<div style="flex: 1 0 60%;"><img src="' + e.target.src + '" alt="' + e.target.alt + '" style="display: block; height: auto; max-width: 100%; margin: 0 auto;"></div><div style="flex: 1 0 40%;"><div style="font-size: 18px; font-weight:bold;">' + e.target.dataset.name + '</div>Цена:<br><b>' + e.target.dataset.price + '$</b></div>');
      modal.show();
    } else if (e.target.dataset.handler === 'modalHandlerCancel') {
      modal.hide();
    } else if (e.target.dataset.handler === 'modalHandlerDelete') {
      elemTarget.parentElement.removeChild(elemTarget);
      modal.hide();
    }
  });
})();
</script>
5. Пример, в котором создано 2 разных модальных окна. Первое модальное окно открывается при нажатии на одни элементы, а второе – при клике на другие:

<button class="btn" data-toggle="modal-1">Открыть окно 1</button>
<button class="btn" data-toggle="modal-1">Открыть окно 1</button>  
<button class="btn" data-toggle="modal-2">Открыть окно 2</button>
<button class="btn" data-toggle="modal-2">Открыть окно 2</button> 

<script>
// создадим модальное окно 1
var modal1 = $modal({
  title: 'Модальное окно 1',
  content: 'Содержимое модального окна 1'
});
// создадим модальное окно 2
var modal2 = $modal({
  title: 'Модальное окно 2',
  content: 'Содержимое модального окна 2'
});
// при клике по кнопке #show-modal-1
document.addEventListener('click', function (e) {
  if (e.target.dataset.toggle === 'modal-1') {
    // отобразим модальное окно N1
    modal1.show();
  } else if (e.target.dataset.toggle === 'modal-2') {
    // отобразим модальное окно N2
    modal2.show();
  }
});
</script>
6. Пример всплывающего окна, данные в которое загружаются с использованием AJAX:

<a href="#" data-json="examples/json/json-1">из json-1</a>
<a href="#" data-json="examples/json/json-2">из json-2</a>
...
<script>
// создадим модальное окно
var modal = $modal({
  title: 'Модальное окно',
});
// при клике по документу
document.addEventListener('click', function (e) {
  // если элемент имеет атрибут data-json, то...
  if (e.target.dataset.json) {
    e.preventDefault();
    // выполним AJAX запрос на сервер по адресу определяемым значением атрибута data-json
    var request = new XMLHttpRequest();
    request.open('GET', 'https://itchief.ru/' + e.target.dataset.json);
    request.send();
    request.onload = function () {
      if (request.status == 200) {
        // если запрос успешный, то обработаем полученный ответ (который нам, например, приходит в формате JSON) и сформируем контент, который затем установим в качестве соержимого модального окна  
        var
          data = JSON.parse(request.response),
          content = '<div style="flex: 1 0 60%;"><img src="{{image}}" alt="" style="display: block; height: auto; max-width: 100%; margin: 0 auto;" width="705" height="440"></div><div style="flex: 1 0 40%;"><div style="font-size: 18px; font-weight:bold;">{{title}}</div>Цена:<br><b>{{price}}</b></div>',
          result;
        result = content.replace('{{title}}', data.title);
        result = result.replace('{{price}}', data.price);
        result = result.replace('{{image}}', data.image);
        modal.setContent(result);
        // отобразим модальное окно
        modal.show();
      }
    };
  }
});
</script>
Пример содержимого файла «json-1»:

{"title":"Audi A5 Coupé","price":"22500$","image":"https://itchief.ru/examples/images/car-1.jpg"}
7. Этот пример содержит код для обработки различных событий, связанных с модальном окном и кнопками, расположенными в нём:

<!-- Кнопки для открытия модального окна -->
<button id="show-1" class="show" data-toggle="modal">show-1</button>
<button id="show-2" class="show" data-toggle="modal">show-2</button>
<!-- Элементы, которые будем использовать для вывода различных сообщений связанных с изменениями состояния модального окна -->
<div class="message"></div>
<div class="actions"></div>

...
<script>
(function () {
  var elemTarget;
  var modal = $modal({
    title: 'Текст заголовка',
    content: '<p>Содержмиое модального окна...</p>',
    footerButtons: [
      { class: 'btn btn-2', text: 'ОК', handler: 'modalHandlerOk' },
      { class: 'btn btn-1', text: 'Отмена', handler: 'modalHandlerCancel' }
    ]
  });
  document.addEventListener('show.modal', function (e) {
    document.querySelector('.actions').textContent = 'Действия при открытии модального окна...';
    // получить ссылку на DOM-элемент показываемого модального окна (.modal)
    console.log(e.detail);
  });
  document.addEventListener('hide.modal', function (e) {
    document.querySelector('.actions').textContent = 'Действия при закрытии модального окна...';
    // получить ссылку на DOM-элемент скрываемого модального окна (.modal)
    console.log(e.detail);
  });
  document.addEventListener('click', function (e) {
    if (e.target.dataset.toggle === 'modal') {
      elemTarget = e.target;
      modal.show();
      modal.setContent('Вы открыли модальное окно посредством нажатия на кнопку <b>' + e.target.textContent + '</b>');
    } else if (e.target.dataset.handler === 'modalHandlerCancel') {
      modal.hide();
      document.querySelector('.message').textContent = 'Вы нажали на кнопку Отмена, а открыли окно с помощью кнопки ' + elemTarget.textContent;
    } else if (e.target.dataset.handler === 'modalHandlerOk') {
      modal.hide();
      document.querySelector('.message').textContent = 'Вы нажали на кнопку ОК, а открыли окно с помощью кнопки ' + elemTarget.textContent;
    } else if (e.target.dataset.dismiss === 'modal') {
      document.querySelector('.message').textContent = 'Вы закрыли модальное окно нажав на крестик или на область вне модального окна, а открыли окно с помощью кнопки ' + elemTarget.textContent;
    }
  });
})();
</script>
Описание скрипта модального окна
В этом разделе приведена информация для тех, кто хочет более подробно разобраться с тем, как работает это модальное окно.

Её JavaScript код представлен посредством функции $modal:

$modal = function (options) {
  var
    _elemModal,
    _eventShowModal,
    _eventHideModal,
    _hiding = false,
    _destroyed = false,
    _animationSpeed = 200;

  function _createModal(options) { ... }
  function _showModal() { ... }
  function _hideModal() { ... }
  function _handlerCloseModal(e) { ... }

  _elemModal = _createModal(options || {});
  _elemModal.addEventListener('click', _handlerCloseModal);
  _eventShowModal = new CustomEvent('show.modal', { detail: _elemModal });
  _eventHideModal = new CustomEvent('hide.modal', { detail: _elemModal });

  return {
    show: _showModal,
    hide: _hideModal,
    destroy: function () { ... },
    setContent: function (html) { ... },
    setTitle: function (text) { ... }
  }
};
В качестве результата эта функция возвращает объект, состоящий из 5 методов. Они позволяют нам выполнять различные действия над созданным модальным окном. Назначение каждого метода, а также различные примеры как их использовать мы уже подробно рассмотрели выше. Здесь мы более подробно разберём внутренние переменные и функции $modal.

В $modal имеются следующие переменные _elemModal, _eventShowModal, _eventHideModal, _hiding, _destroyed, _animationSpeed и функции _createModal, _showModal, _hideModal, _handlerCloseModal.

Функция _createModal предназначена для формирования HTML-кода модального окна (DOM структуры) и добавления её на страницу. В качестве результата она возвращает ссылку на базовый элемент этого модального окна. Т.к. нам эта ссылка нужна в других частях $modal, то сохраним её в переменную _elemModal:

_elemModal = _createModal(options || {});
Переменные _eventShowModal и _eventHideModal применяются для хранения созданных нами кастомных событий «show.modal» и «hide.modal». Событие «show.modal» мы будем вызывать при открытии модального окна, а «hide.modal» – при закрытии. Эти события будем генерировать для объекта document. Используя их, вы можете очень просто добавить свою логику при открытии и закрытии модального окна:

document.addEventListener('show.modal', function (e) {
  // в e.detail содержится ссылка на открываемое модальное окно
  ...
});
document.addEventListener('hide.modal', function (e) {
  // в e.detail находится ссылка на корневой DOM-элемент закрываемого модального окна
  ...
});
Переменные _hiding и _destroyed используются для хранения состояний. Первая применяется для индикации процесса скрытия модального окна. Она имеет значение true во время скрытия окна, в остальных моментах - false. Вторая переменная хранит true или false, в зависимости от того, удалены ли DOM элементы модального окна со страницы и связанные с ним события или нет.

Переменная _animationSpeed используется для указания времени длительности процесса скрытия модального окна (в миллисекундах).

Функция _showModal предназначена для включения отображения модального окна на странице, а _hideModal – для его скрытия.

Функция _handlerCloseModal используется в качестве обработчика события «click» для документа и выполняет скрытие модального окна при нажатии на кнопку его закрытия или вне его.