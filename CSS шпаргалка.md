CSS шпаргалка

#селекторы

.<имя_класса> {
	
}

#<id_elem> {
	
}

div (tagname) {
	
}

<div> растягивается на всю ширину экрана по умолчанию, размещение соседних элементов по умолчанию идет блоками друго под другом, высота по умолчанию зависит от контента
<span> строчный элемент, ширина зависит от контента, расположение друг за другом на одной строке


# отступы

padding отступ от внутренних границ внутри блока
margin отступ от внешних элементов

# разместить элемент по центру:

{
	margin: 0 auto; /*разместить элемент по центру*/
	margin: 0 /*убрать границы между элементами*/
}

# если контент выходит за границы блока фиксированных размеров

{
	overflow:hidden /*скрывает содержимое*/
	overflow:scroll /*добавляет со всех сторон полосу прокрутки*/
	overflow:auto /*автоматически вычисляет где нужна полоса прокрутки*/
}

# display способ отображения

{
	display: inline-block /*выстраивает блочные элементы в строку*/
	display: block /*выстраивает элемент блочным типом (друг под другом)*/
	display: inline /*выстраивает блочные элементы в строку при этом они становятся строчными (нельзя манипулировать высотой, шириной)*/
}

# position позиционирование

{
	position:static; /*по умолчанию*/

	position:fixed; /*фиксированная позиция после позициронирония, блок неподвижен при изменении границ окна, например при прокрутке*/

	position:absolute; /*абсолютная, то есть относительно самой страницы, или границ окна, не учитывая других элементов*/

	position:relative; /*относительно его исходной позиции и других соседних элементов*/

	left: 20px; /*отодвигает элемент на 20 пикселей относительно другого элемента слева, согласно заданному позициронированию, если absolute то относительно границ окна*/

	z-index: 1; /*положение элемента по отношению к другим по перпендикулярной оси Z, т.е. как бы возвышает или утапливает элемент относительно других, если элемент фиксированный, это нужно, чтобы он возвышался над другими, иначе другие элементы могут наложиться на него, значение z-index зависит от того, есть ли другие элементы с заданной позицией и необходимо учитывать это, так как элементы с одинаквоым z-index будут накладываться друг на друга*/
}

если есть вложенный div в div и в дочерним div нужно позиционировать абсолютно относительно родительского, то

в **родительском** нужно указать:

{
	position: relative;
}

в дочернем:

{
	position: absolute;
}

#float 
{
	float: left; /*позиционирование по левой стороне, также задает обтекание, родительский блок перестает подстраиваться по размер под элементы, как бы "выгоняет" их, если в родительском блоке задать overflow: hidden, то float элементы снова вернутся в границы родительского*/

	clear: both; /*отменяет обтекание со всех сторон*/
}

#задание фона из изображения в отдельном файле

{
	background: url("../path/to/image.png"); /*заполняет блок картинкой по всей ширине, если размер картинки меньше блока, то размножает картинку пока не заполнит весь блок*/
	background: url("../path/to/image.png") no-repeat; /*заполняет блок картинкой без повторения*/
	background: url("../path/to/image.png") repeat-x; /*заполняет блок картинкой полностью по оси x*/
	background: url("../path/to/image.png") no-repeat red; /*заполняет блок картинкой без повторения и оставшуюся незаполненную часть цветом*/
}

#если нужно чтобы в блоке html текст переносился по строкам и учитывалась табуляция

{
	white-space: pre-wrap;
}