# P4.-Network-e-Compose

<h2>1. Crear unha rede en Docker</h2>
<p>Para crear unha rede en Docker, usamos o comando <code>docker network create</code>
para crear unha rede personalizada. Neste caso, chamaremos á rede <code>mi_red</code>:</p>
<p><code>bash
docker network create mi_red
</code></p>
<h2>2. Crear dous contenedores unidos a esa rede</h2>
<p>Agora creamos dous contenedores e os unimos á rede <code>mi_red</code>. Usaremos a
imaxe <code>nginx</code> como exemplo:</p>
<p><code>bash
docker run -d --name contenedor1 --network mi_red nginx
docker run -d --name contenedor2 --network mi_red nginx
</code></p>
<h2>3. Comprobar que os contenedores están na rede</h2>
<p>Para comprobar que os contenedores están na rede <code>mi_red</code>, usa o
comando:</p>
<p><code>bash
docker network inspect mi_red
</code></p>
<p>Este comando amosará a información sobre a rede, incluíndo os contenedores que están
conectados a ela.</p>
<h2>4. Comprobar que os contenedores poden verse entre eles</h2>
<p>Para verificar que os contenedores se poden comunicar entre eles, accede a un contenedor e
fai un <code>ping</code> ao outro:</p>
<p><code>bash
docker exec -it contenedor1 bash
ping contenedor2
</code></p>
<p>Se todo está configurado correctamente, deberían poderse comunicar.</p>
<h2>5. Listar os contenedores conectados á rede</h2>
<p>Para listar todos os contenedores conectados á rede <code>mi_red</code>:</p>
<p><code>bash
docker network inspect mi_red
</code></p>
<h2>6. Listar as propiedades da rede</h2>
<p>Para ver as propiedades da rede, usa o mesmo comando <code>docker network
inspect</code>:</p>
<p><code>bash
docker network inspect mi_red
</code></p>
<p>Este comando devolverá información sobre a rede, como o driver que está a usar (normalmente
<code>bridge</code>) e os contenedores conectados.</p>
<h2>7. Crear outra rede</h2>
<p>Podemos crear outra rede chamada <code>mi_red2</code> de maneira similar:</p>
<p><code>bash
docker network create mi_red2
</code></p>
<h2>8. Lanzar dous contenedores novos conectados a esa nova rede</h2>
<p>Agora lanzamos dous novos contenedores conectados á nova rede
<code>mi_red2</code>:</p>
<p><code>bash
docker run -d --name contenedor3 --network mi_red2 nginx
docker run -d --name contenedor4 --network mi_red2 nginx
</code></p>
<h2>9. Comprobar as posibles conexións entre os 4 contenedores</h2>
<p>Podemos comprobar se os contenedores das redes <code>mi_red</code> e
<code>mi_red2</code> se poden comunicar entre eles. Como están en redes separadas, non
deberían poderse comunicar entre si sen configuración adicional. Sen embargo, se quixeramos
permitir a comunicación entre redes, deberíamos usar algún tipo de configuración como
<code>docker-compose</code> ou configurar enlaces específicos entre redes.</p>
<h2>10. Docker Compose</h2>
<h3>Guía de iniciación de Docker Compose</h3>
<p>Docker Compose é unha ferramenta que permite xestionar aplicacións multi-contedor. A
continuación, explico os pasos principais:</p>
<ol>
<li><p><strong>Instalar Docker Compose</strong> (se non o tes instalado):</p>
<p><code>bash
sudo apt-get install docker-compose
</code></p></li>
<li><p><strong>Crear un arquivo <code>docker-compose.yml</code></strong>.</p>
<p>A continuación, crearás un arquivo <code>docker-compose.yml</code> para definir os
contenedores e redes.</p></li>
<li><p><strong>Lanzar a aplicación</strong>:</p>
<p>Unha vez creado o arquivo <code>docker-compose.yml</code>, usa o seguinte comando para
iniciar os contenedores definidos no arquivo:</p>
<p><code>bash
docker-compose up
</code></p></li>
</ol>
<h3>Exemplo de <code>docker-compose.yml</code></h3>
<p>A continuación, imos crear un arquivo <code>docker-compose.yml</code> que defina tres
contenedores conectados á mesma rede <code>mi_red</code>:</p>
<p>```yaml
version: '3'
services:
contenedor1:
image: nginx
networks:
- mi<em>red
contenedor2:
image: nginx
networks:
- mi</em>red
contenedor3:
image: nginx
networks:
- mi_red</p>
<p>networks:
mi_red:
driver: bridge
```</p>
<p><strong>Explicación:</strong>
- <strong><code>version: '3'</code></strong>: Define a versión de Docker Compose.
- <strong><code>services</code></strong>: Define os contenedores que se van a crear, que neste
caso son <code>contenedor1</code>, <code>contenedor2</code> e <code>contenedor3</code>.
Todos usan a imaxe <code>nginx</code> e están conectados á rede <code>mi_red</code>.
- <strong><code>networks</code></strong>: Define a rede <code>mi_red</code> que se usará
para os contenedores.</p>
<p>Para lanzar os contenedores con Docker Compose:</p>
<p><code>bash
docker-compose up -d
</code></p>
<h3>Parámetros adicionais en Docker Compose</h3>
<ol>
<li><p><strong><code>build</code></strong>: Permite construír unha imaxe a partir dun
<code>Dockerfile</code>.</p>
<p><code>yaml
build: .
</code></p>
<p>Isto indica que a imaxe debe construírse desde o <code>Dockerfile</code> no directorio
actual.</p></li>
<li><p><strong><code>volumes</code></strong>: Monta un volume para persistir datos entre
reinicios do contenedor.</p>
<p>```yaml
volumes:</p>
<ul>
<li>./mi_datos:/data
```</li>
</ul>
<p>Isto montará o directorio local <code>./mi_datos</code> dentro do contenedor no directorio
<code>/data</code>.</p></li>
<li><p><strong><code>ports</code></strong>: Mapea os portos entre o contenedor e a máquina
anfitriona.</p>
<p>```yaml
ports:</p>
<ul>
<li>"8080:80"
```</li>
</ul>
<p>Isto mapea o porto 80 do contenedor ao porto 8080 da máquina anfitriona.</p></li>
<li><p><strong><code>environment</code></strong>: Define variables de ambiente dentro do
contenedor.</p>
<p>```yaml
environment:</p>
<ul>
<li>NOMBRE=MiContenedor
```</li>
</ul>
<p>Isto establece a variable de ambiente <code>NOMBRE</code> dentro do contenedor.</p></li>
</ol>
<h2>Resumo</h2>
<p>Con Docker Compose, podes xestionar múltiples contenedores conectados a redes definidas
no arquivo <code>docker-compose.yml</code>. Este arquivo permite personalizar a configuración
dos contenedores e as redes de forma sinxela e ordenada.</p>
