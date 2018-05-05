---


---

<h2 id="undefined-behavior">Undefined Behavior</h2>
<h3 id="aliasing">Aliasing</h3>
<pre><code>void sum(float* result, float const* arr, size_t n) {
	for(size_t i = 0; i != n; ++i) {
		*result += arr[i];
	} 
}
</code></pre>
<p>Компилятор генерирует код, в котором <code>result</code> все время перезаписывается в память, из которой никогда не будет читать, потому что в <code>result</code> можно передать один из элементов массива.</p>
<pre><code>void memcpy(char* dst, char const* src, size_t size) {
	for(size_t i = 0; i != size; ++i) {
		dst[i] = src[i];
	} 
}
</code></pre>
<p>Если память пересекается, то на других инпутах результат функции может быть совсем иным, поэтому компилятор должен сохранить функцию в рабочем состоянии.</p>
<p><code>__restrict</code> - игнорируем алиасинг.</p>
<p>Если передать в <code>__restrict</code> алиасищиеся указатели, то вообще хз что будет</p>
<p><strong>Undefined Behavion</strong> - компилятор имеет право предполагать, что такой ситуации не бывает.</p>
<h3 id="классека">Классека</h3>
<pre><code>void f(float* p) {
	float unused = *p  // Разыменование нулевого указателя
	
	if (!p) {
		return;
	}

	printf("%f", *p);
}
</code></pre>
<p>Разыменование нулевого указателя - это <strong>UB</strong>, можно убрать все кроме принта.</p>
<pre><code>bool f(int a) {
	return (a + 1) &lt; a;
}
</code></pre>
<p>Тождественный <code>false</code> из-за <strong>UB</strong> (переполнение).</p>
<p>Сдвиг более чем на битность регистра - <strong>UB</strong>.</p>
<p><strong>Type-based alias analysis/strict aliasing rule</strong> - предположение, что указатели на никакие 2 типа не алиасятся (кроме указателя на один и тот же тип с точностью до <code>unsigned</code> + <code>char</code> может алиасить любой другой тип (фиксится структуркой с чаром)).</p>
<p><code>-fno-strict-aliasing</code> - отключить данное предположение.<br>
<code>-fsanitize=undefined</code><br>
<code>-fsanitize=address</code></p>

