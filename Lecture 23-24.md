---


---

<h2 id="stl">STL</h2>
<p><strong>Elements of Programming</strong> - книга</p>
<h3 id="части">Части</h3>
<ol>
<li>Контейнеры (<strong>Containers</strong>)
<ul>
<li>Sequences (vector, list, deque)</li>
<li>Associative ([unordered] [multi] set, map)</li>
</ul>
</li>
<li>Итераторы (<strong>Iterators</strong>)
<ul>
<li>*<strong>it</strong> (взять элемент, на который сейчас ссылается итератор)</li>
<li><strong>==/ !=</strong> (ссылаются ли на один и тот же элемент)</li>
<li><strong>++it</strong> (следующий элемент)</li>
<li><strong>begin</strong> (первый элемент)</li>
<li><strong>end()</strong> (следующий за последним)</li>
<li>Классы
<ul>
<li><strong>Input</strong> (можно прочитать 1 раз)</li>
<li><strong>Output</strong> (можно вывести 1 раз)</li>
<li><strong>Forward</strong> (пройти вперед можно 1 раз, но можно сделать копию и повторить). Например <strong>search, rotate</strong>.</li>
<li><strong>Bidirectional</strong> (имеет операции <strong>–it</strong>). Например <strong>list, reverse</strong></li>
<li><strong>Randomaccess</strong>(-//- <strong>it+5, it-5, it&lt;it2, it2-it</strong>, ect). Например <strong>sort</strong>.</li>
</ul>
</li>
</ul>
</li>
<li>Алгоритмы (<strong>Algorithms</strong>)
<ul>
<li><strong>find()</strong></li>
<li><strong>sort()</strong></li>
<li>ect.</li>
</ul>
</li>
<li>Адаптеры контейнеров (<strong>Container Adapter</strong>)
<ul>
<li><strong>stack</strong></li>
<li><strong>queue</strong></li>
<li><strong>priority_queue</strong></li>
</ul>
</li>
<li>Функциональные объекты (<strong>Functional object</strong>)
<ul>
<li>Такое <strong>f()</strong>, что его можно вызвать. Н-р функция.</li>
<li><code>bool operator()(T const a, T const b){...}</code></li>
</ul>
</li>
</ol>
<h3 id="подробнее">Подробнее</h3>
<p><code>vector&lt;T&gt;, list&lt;t&gt;, deque&lt;T&gt;</code> требует от T:</p>
<ol>
<li>Дефолтный конструктор</li>
<li><code>T(T const&amp;)</code></li>
<li><code>~T()</code></li>
<li><code>operator =</code></li>
</ol>
<p><code>set&lt;T&gt;/map&lt;T&gt;</code> еще <code>operator&lt;()</code></p>
<h4 id="algorithms">Algorithms</h4>
<pre><code>template&lt;typename InputIterator,
		 typename Predicate&gt;
InputIterator find_if(InputIterator first
					  InputIterator last,
					  Predicate pred)
{
	while (first != last &amp;&amp; !pred(*first)) {
		++first;
	}
	return first;
}

struct divible_by {
	divible_by(int divible) {
		divisor = divible;
	}
	bool operator()(T div) {
		return div % divisor == 0;
	}
	private:
		int divisor;
}

	int main() {
		int a[] = {...};
		int *pos = find_if(begin(a),
						   end(a),
						   divible_by(3))
	}
</code></pre>
<h3 id="полиморфизм">Полиморфизм</h3>
<pre><code>bool int_less(int a, int b) {
	return a &lt; b;	
}

	sort(v.begin(), v.end(), std::less&lt;int&gt;()) - static
	sort(v.begin(), v.end(), &amp;int_less()) - dinamic
</code></pre>
<p>Второе очень плохо оптимизируется и работает гораздо дольше.</p>
<p><strong>Profile guided optimization</strong>:</p>
<ul>
<li><code>-fprofile-generate</code> - собирает статистику работы программы.</li>
<li><code>-fprofile-use</code> - Использует полученную информацию для оптимизации, иногда начинает работать быстрее.</li>
</ul>
<p><strong>SFINAE - sub</strong></p>

