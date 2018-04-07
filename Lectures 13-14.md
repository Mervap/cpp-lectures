---


---

<pre><code>struct base {
    virtual void print() {
	    std::cout &lt;&lt; "base\n";
    } 
}

struct derived : base {
    virtual void print() {
	    std::cout &lt;&lt; "derived\n";
    } 
}

void main() {
    derived d;
    base b = d;  // base b((const base&amp;) d) - Слайсинг?
			     // (Вызов конструктора копирования)
    b.print();   // derived
    ((base&amp;)d).print(); // derived

	base&amp; b = d;
	b.print()	 // base
	((base)d).print(); // base
}
</code></pre>
<h1 id="exceptions-">Exceptions (!)</h1>
<p>Можно, но много мусорного кода</p>
<pre><code>bool do_something() {
    FILE* file = fopen("1.txt");
    if (!file) {
	    return false;
    }

	ssize_t bytes_read = fread(..., file);
	if (bytes_read &lt; 0) {
		return false;
	}

	fclose(file);
	return true;
}
</code></pre>
<p>Поэтому есть исключения</p>
<pre><code>void f() {
    if (...)
	    throw runtime_error("f() failed");
}
</code></pre>
<p>Тоже завершает f(), но с ошибкой и возвращает эту ошибку по call stack’у</p>
<pre><code>int main {
	try {
		...
		f();
		...
	} catch (runtime_error const&amp; e) {
		// Проверяет что ошибка соответствует типу (или базового)
		// Если ни один catch подошел, то ошибка передается выше
	} catch (...) {
		// Любой exception, специальная форма
		throw; // Пробросить exception дальше
	}
}
</code></pre>
<p>Примерчик</p>
<pre><code>struct base {
    virtual std::string msg() {
	    return "base";
    } 
}

struct derived : base {
    std::string msg() {
	    return "derived";
    } 
}

void main() {
    try {
	    throw derived();
    } catch (base const&amp; e) {
	    e.msg(); //derived
    } catch (base e) {
	    e.msg(); //base
    }
}
</code></pre>
<p>Из main бросать исключение нельзя<br>
А теперь вложенные</p>
<pre><code>void main() {
    try {
	    try {
		    throw derived();
	    } catch (base const&amp; e1) {
		    e1.msg();
					  // Без throw - derived
		    throw; 	  // derived, derived
		    throw e1; // derived, base
	    }
    } catch (base const&amp; e2) {
	    e2.msg();
    }
}
</code></pre>
<p>throw e1 - бросает значение статического типа<br>
Динамический - throw;</p>
<h3 id="что-исключение--а-что-нет">Что исключение,  а что нет?</h3>
<p>Если мы захотели прервать какое-то вычисление, то гораздо удобнее вместо bool и if использовать exception, хотя прерывание вычислений может и совсем не являться ошибкой</p>
<p>Exception - надеемся, что будет передаваться наверх какое-то количество функций</p>
<h2 id="использование-и-последствия">Использование и последствия</h2>
<p>Не закрываются файлы в случае ошибки чтения</p>
<pre><code>bool do_something() {
    FILE* file = fopen("1.txt");
    
    fread(..., file);
    fread(..., file);
    fread(..., file);
	
	fclose(file);
}
</code></pre>
<p>Фиксится деструктором<br>
RAIL - resource allocation is initialization</p>
<pre><code>struct file_descriptor {		

	//Запретить дефолтный конструктор		

	file_descriptor (std::string cons&amp; file_name) {
		file = fopen(file_name);
	} 
	
	read() {
		//...
	}
}

bool do_something() {
    file_descriptor f("1.txt");
  
    f.read();
	f.read();	    	    	    
	f.read();
}
</code></pre>
<p>Замена дефолтного конструктора, списки инициализации</p>
<pre><code>struct config_file {
	config_file() 
		: f1("1.txt")
		, f2("2.txt")
		, f3("3.txt")
	{
		// ...
	}

	file_descriptor f1;
	file_descriptor f2;
	file_descriptor f3;
}
</code></pre>
<p>Запрет на неявные приведения - <strong>explicit</strong></p>
<pre><code>template&lt;typename T&gt;
explicit unique_ptr(T* p) : p(p) {}
</code></pre>
<h3 id="гарантии-исключений">Гарантии исключений:</h3>
<ul>
<li>Никогда не бросает исключений</li>
<li>Строгая гарантия (состояние объекта не меняется)</li>
<li>Слабая гарантия (класс остается в некотором неспецефицированном, но корректном состоянии. Например обнуление строчки</li>
<li>Никакой гарантии (некорректный код)</li>
</ul>
<p><strong>Swap-trick</strong>  - строгая гарантия исключений</p>
<pre><code>void swap(string&amp; other) {
	std::swap(data, other.data);
	std::swap(size, other.size);
	std::swap(capacity, other.capacity);	
}

string&amp; operator=(string const&amp; str) {
	string s(str);
	swap(s);
	return *this;
}
</code></pre>

