---


---

<h2 id="templates">Templates</h2>
<p>Например хотим контейнер для любого типа элементов</p>
<ol>
<li>
<p>Можно использовать <code>void*</code> - в нем храним все что угодно</p>
</li>
<li>
<p>Можно еще так:</p>
<pre><code> #define DECLARE_VECTOR(type)
 struct vector_##type {
     ...
 }
</code></pre>
</li>
</ol>
<ul>
<li>У первого меньше размер объектных файлов</li>
<li>Зато можно дебажить</li>
<li>И компилируется быстрее (по сравнению с активным использованием 2 метода)</li>
<li>Но нет информации о типе</li>
<li>Но завалить все <code>void*</code> крайне неэффективно</li>
</ul>
<p>Специально для этих целей <code>template</code></p>
<pre><code>template &lt;typename T&gt;
struct vector {
    T* data;
    size_t size;
    size_t capacity;
}

template &lt;typename T&gt; 
T const&amp; max(T const&amp; a, T const&amp; b) {
	return a &lt; b ? b : a;
}

max(1, 2);
max&lt;int&gt;(1, 2); //Указывает какой конкретный тип
</code></pre>
<p>Для линковщика <code>template</code> функции помечаются как <code>inline</code><br>
<code>Template</code> функции обычно описываются сразу в <code>header</code> файле</p>
<h3 id="примерчики">Примерчики</h3>
<p><code>mytype a(b)</code> - если <code>b</code> - тип, то <code>a</code> - функция, иначе - переменная</p>
<p><code>mytype a(T::foo), T.foo(b), (T::foo)-b</code> - не знаем каст или операция вычитания<br>
Поэтому если не написано <code>typename</code>, то считается  НЕ типом<br>
<code>(T::foo) - b</code> - вычитание<br>
<code>(typename T::foo) - b</code> - каст</p>
<p><code>a &lt; b &gt; c</code> - <code>(a &lt; b) &gt; c</code> или  <code>a&lt;b&gt; c</code>?<br>
`T::template foo<b> c</b></p>
<h2 id="специализация">Специализация</h2>
<p>Primary template</p>
<pre><code>template &lt;typename T&gt; 
struct vector{};
</code></pre>
<p>Explicit specialization</p>
<pre><code>template&lt;&gt;
struct vector&lt;bool&gt;{}; // Отдельная реализация для bool
</code></pre>
<p>Partical specialization</p>
<pre><code>template&lt;typename U&gt;
struct vector&lt;U*&gt;{}; // Отдельная реализация для указателей
</code></pre>
<p>А что если подходит под 2?</p>
<pre><code>template&lt;typename T&gt;
struct mytype{}; 

template&lt;typename U&gt;
struct mytype&lt;U*&gt;{}; 

template&lt;typename V&gt;
struct mytype&lt;V**&gt;{}; 

mytype&lt;int&gt;;   // 1-ое
mytype&lt;int*&gt;;  // 2-ое
mytype&lt;int**&gt;; // Подходит и 2-ое и 3-е
</code></pre>
<p>В таких случаях выбирается более специализированное, т.е выберется 3-е</p>
<pre><code>template&lt;typename T, typename P&gt;
struct mytype{}; 

template&lt;typename U, typename Q&gt;
struct mytype&lt;U*, Q&gt;{}; 

template&lt;typename V, typename R&gt;
struct mytype&lt;V, R*&gt;{}; 

mytype&lt;int, int&gt;;   // 1-ое
mytype&lt;int*, int&gt;;  // 2-ое
mytype&lt;int, int*&gt;;  // 3-е
mytype&lt;int*, int*&gt;; // Ошибка, подходит и 2 и 3, но нет более специализированной
</code></pre>
<p>Можно так</p>
<pre><code>template&lt;typename V&gt;
struct mytype&lt;V, int&gt;{}; // Больше параметров

template&lt;typename U, typename V&gt;
struct mytype&lt;pair&lt;U, V&gt;&gt;{}; // Меньше параметров
</code></pre>
<p>Запретить просто оставить без тела</p>
<pre><code>template&lt;typename T&gt;
struct mytype;
</code></pre>
<p>Иногда может потребоваться передать значение</p>
<pre><code>std::array&lt;int, 10&gt;
template&lt;typename T, site_t N&gt;
struct array{};
</code></pre>
<p>Может быть любой целочисленный <strong>nontype</strong> параметр</p>
<p>Специализации тут тоже работают</p>
<pre><code>template&lt;typename T&gt;
struct array&lt;T, 0&gt;{};
</code></pre>

