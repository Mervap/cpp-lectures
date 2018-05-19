---


---

<h2 id="c---style-casts">C++ - style casts</h2>
<p><code>(type) expr</code> - каст</p>
<pre><code>struct base {
	virtual ~base();
}

struct derived : base {
	void g();
}

void f(base&amp; obj) {
	if(typeid(obj) == typeid(derived)) {
		derived&amp; d = (derived&amp;)obj; // Корректный
		d.g();
	}
}
</code></pre>
<p>Но если определение функции будет после <code>f</code>, каст будет уже некорректным</p>
<ol>
<li>
<p><code>static_cast&lt;type&gt;(expr);</code> позволяет кастить:</p>
<ul>
<li><strong>int</strong> &lt;-&gt; <strong>int</strong></li>
<li><strong>base</strong> &lt;-&gt; <strong>derived</strong></li>
<li><strong>type</strong>* &lt;-&gt; <strong>void</strong>*</li>
</ul>
</li>
<li>
<p><code>reinterpret_cast</code> - используется для каста указателя в число и наоборот (не очень корректная штука вообще)</p>
</li>
<li>
<p><code>const_cast</code> - снятие <code>const</code> (а это вообще может привести к <strong>UB</strong>)</p>
<pre><code> int main() {
 	 const int a = 5;
 	 const_cast&lt;int&amp;&gt;(a) = 6;
 	 cout &lt;&lt; a * a; // Выводит 25
  }
</code></pre>
</li>
<li>
<p><code>dynamic_cast</code> - если не уверены что каст структуры возможен (?)</p>
<pre><code>struct base {
    virtual ~base();
}
   	
struct derived : base {
}
   	
void f(base* b) {
    if(derived* d = dynamic_cast&lt;derived*&gt;(b)) 
    {}
}
</code></pre>
</li>
</ol>
<h2 id="profilers">Profilers</h2>
<p>Позволяет посмотреть где программа проводит свое время<br>
<strong>perf</strong> - встроен в linux, <code>perf record/report</code></p>
<p><strong>intel vtune amplifier</strong></p>

