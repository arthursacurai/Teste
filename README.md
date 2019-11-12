<p>Para subir a aplicação com a biblioteca Flask é necessário criar uma instância da mesma, que no caso chamei de &quot;app&quot;. Como parâmetro deve ser passado o nome do módulo para que o Flask saiba onde procurar por templates, arquivos estáticos, etc. Porém nenhuma função ou parâmetro foi passado, para isso deve-se usar o decorator route.</p>
<pre><code class='language-python' lang='python'>from flask import Flask, render_template, request, redirect
from flask_mysqldb import MySQL
import yaml

app = Flask(__name__)

db = yaml.load(open(&#39;db.yaml&#39;))
app.config[&#39;MYSQL_HOST&#39;] = db[&#39;mysql_host&#39;]
app.config[&#39;MYSQL_USER&#39;] = db[&#39;mysql_user&#39;]
app.config[&#39;MYSQL_PASSWORD&#39;] = db[&#39;mysql_password&#39;]
app.config[&#39;MYSQL_DB&#39;] = db[&#39;mysql_db&#39;]

mysql = MySQL(app)


@app.route(&#39;/&#39;, methods=[&#39;GET&#39;, &#39;POST&#39;])
def index():
    if request.method == &#39;POST&#39;:
        clienteInfo = request.form
        nome = clienteInfo[&#39;nome&#39;]
        endereco = clienteInfo[&#39;endereco&#39;]
        telefone = clienteInfo[&#39;telefone&#39;]
        email = clienteInfo[&#39;email&#39;]
        cur = mysql.connection.cursor()
        cur.execute(&quot;INSERT INTO user(NOME, ENDERECO, TELEFONE, EMAIL) VALUES(%s, %s, %s, %s)&quot;, (NOME, ENDERECO, TELEFONE, EMAIL))
        mysql.connection.commit()
        cur.close()
        return redirect(&#39;/&#39;)
    return render_template(&#39;index.html&#39;)

if __name__ == &quot;__main__&quot;:
    app.run(debug=True)
</code></pre>
<pre><code class='language-html' lang='html'>&lt;form&gt;
	Nome &lt;input type=&quot;text&quot; name=&quot;nome&quot; /&gt;
	&lt;br&gt;
	Endereco &lt;input type=&quot;text&quot; name=&quot;endereco&quot; /&gt;
	&lt;br&gt;
	Telefone &lt;input type=&quot;tel&quot; name=&quot;telefone&quot; /&gt;
	&lt;br&gt;
	Email &lt;input type=&quot;email&quot; name=&quot;email&quot; /&gt;
	&lt;br&gt;
	&lt;input type=&quot;submit&quot;&gt;
&lt;/form&gt;
</code></pre>
<pre><code class='language-mysql' lang='mysql'>CREATE DATABASE Aplicativo;
USE Aplicativo
CREATE TABLE cliente(
	NOME VARCHAR(40),
	ENDERECO VARCHAR(40),
	TELEFONE VARCHAR(20),
    EMAIL VARCHAR(40)
	);
</code></pre>
<p>Após adicionar o cliente, é possível verificar se os dados foram computados através do código:</p>
<pre><code>SELECT * FROM cliente;
</code></pre>
<p>&nbsp;</p>
<pre><code class='language-yaml' lang='yaml'>mysql_host: &#39;localhost&#39;
mysql_user: &#39;root&#39;
mysql_password: &#39;#######&#39;
mysql_db: &#39;Aplicativo&#39;
</code></pre>
<p>&nbsp;</p>
