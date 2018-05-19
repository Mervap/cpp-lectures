---


---

<h2 id="namespaces">Namespaces</h2>
<p><code>gnome_panel_applet_indicators_xxx</code> - длинно, зато нет ошибок линковки<br>
Но можно исправить <code>namespace</code>'ами</p>
<pre><code>namespace gnome {
	namespace panel {
		namespace applet_indicators {
			void f();
			void g() {
				f();
			}
		}
	}
} 

void h() {
	gnome::panel::applet_indicators::f();
} 
</code></pre>
<p><code>::</code> - квалификация<br>
Писать все время тоже долго, можно сделать<br>
<code>using namespace gnome::panel::applet_indicators</code> и вызывать функцию просто <code>f()</code></p>
<p>Поиск происходит примерно так:</p>
<ol>
<li>При квалифицированном поиске, заходим в <code>namespace a</code>, если <code>f()</code> есть - ее и используем, иначе смотрим какие <code>namespace</code> используются в <code>a</code>.</li>
<li>При неквалифицированном идем вверх по дереву <code>namespace</code>'ов до <code>LCA</code> текущего и в котором мы собираемся искать</li>
</ol>
<h3 id="использование">Использование</h3>
<p>В хэдэрах использовать не очень, потому что везде где он будет инклюдиться, будет использоваться <code>namespace</code></p>
<p><strong>using directive</strong></p>
<pre><code>using namespace sdt;

namespace n {
	using namespace std;
	using namespace lalala; // И там и там есть vector
}

n::vector&lt;char&gt; v; // Ошибка на этапе использования
</code></pre>
<p><strong>using declaration</strong></p>
<pre><code>namespace n {
	std::vector; // Объявляет в n vector
}

namespace n {
	using namespace std::vector;
	using namespace lalala::vector; // Ошибка на этапе компиляции
}
</code></pre>
<p>Можно сделать псевдотайпдеф - <strong>type alias</strong></p>
<pre><code>template &lt;typename T&gt;
using myvector = std::vector&lt;T&gt;;
</code></pre>
<p>Еще можно переименовывать</p>
<pre><code>namespace fs = std::filesystem;
fs::path p;
</code></pre>
<p><strong>ADL</strong> - argument_dependent lookup<br>
<strong>Koenig lookup</strong> (???)<br>
namespace n {<br>
struct big_unteger {};<br>
void swap(big_integer &amp;a, big_integer &amp;b);<br>
big_integer operator+(big_integer &amp;a, big_integer &amp;b);<br>
}</p>
<pre><code>int main() {
	n::big_integer a, b;
	swap(a, b);
}
</code></pre>

