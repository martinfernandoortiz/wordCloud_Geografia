<style>
    /* Cambia el tamaño de la fuente para el bloque de código */
    code {
        font-size: 12px;
    }
</style>

# Nube de palabras a partir de un PDF
Generación de una nube de palabras utilizando python y un archivo pdf. En este caso, se utilizo la circular del Congreso de Geografía de Universidades Públicas del 2023 en Argentina. 
La mayor parte del trabajo es curar el texto. Para este ejemplo, se elimino informacion logistica (horarios, nombres de aulas,etc) y nombres de personas. 

**Consulta el notebook** ![aquí](https://github.com/martinfernandoortiz/wordCloud_Geografia/blob/main/wordCloud.ipynb) 

## Algunos ejemplos generados

### WordCloud Circular - 20 Más frecuentes
<style>
    /* Cambia el tamaño de la fuente para el bloque de código */
    code {
        font-size: 12px;
    }
</style>

 ```python
#Forma de Circulo
x, y = np.ogrid[:300, :300]
mask = (x - 150) ** 2 + (y - 150) ** 2 > 130 ** 2
mask = 255 * mask.astype(int)


wc = WordCloud(height = 300, width = 300, background_color="white", repeat=True,
               mask=mask,
               max_font_size = 42, min_font_size = 6,
               #colormap = "magma",
               max_words = 20,
               #contour_width = .1, contour_color = "grey",
               stopwords = stopwords_total)
wc.generate(text)


plt.figure(figsize=(6, 6))
plt.axis("off")
plt.imshow(wc, interpolation="bilinear")

plt.title("Nube con stopwords - 20 palabras", fontsize=12, color="#440154")

# Guarda la figura antes de mostrarla
plt.savefig('/content/drive/MyDrive/datasets_colab/imagenes/circulo20.jpg', bbox_inches='tight', dpi=75)

# Muestra la figura después de guardarla
plt.show()
 ```
![WordCloud Circular - 20 Más frecuentes](https://github.com/martinfernandoortiz/wordCloud_Geografia/blob/main/imagenes/circulo20.jpg "20 más frecuentes")

### WordCloud Argentina

 ```python
#Forma Imagen
mask = np.array(Image.open("/content/drive/MyDrive/datasets_colab/argentina.jpg"))

wc = WordCloud(height = 300, width = 300, background_color="white", repeat=True,
               mask=mask,
               max_font_size = 42, min_font_size = 6,
               #colormap = "magma",
               #max_words = 3,
               #contour_width = .1, contour_color = "grey",
               stopwords = stopwords_total)
wc.generate(text)


plt.figure(figsize=(20, 10))


plt.axis("off")
plt.imshow(wc, interpolation="bilinear")

plt.title("Nube Argentina", fontsize=16, color="#440154")

# Guarda la figura antes de mostrarla
plt.savefig('/content/drive/MyDrive/datasets_colab/imagenes/argentina.jpg', bbox_inches='tight', dpi=75)

# Muestra la figura después de guardarla
plt.show()
 ```

![WordCloud Argentina](https://github.com/martinfernandoortiz/wordCloud_Geografia/blob/main/imagenes/argentina.jpg)

### WordCloud Argentina Dark
 ```python
mask = np.array(Image.open("/content/drive/MyDrive/datasets_colab/argentina.jpg"))

wc = WordCloud(height=300, width=300, background_color="black", repeat=True,
               mask=mask,
               max_font_size=42, min_font_size=6,
               colormap="tab20c",
               stopwords=stopwords_total)
wc.generate(text)

plt.figure(figsize=(20, 10))
plt.axis("off")
plt.imshow(wc, interpolation="bilinear")

plt.title("Nube Argentina Dark", fontsize=16, color="#ff7f0e")

# Guarda la figura antes de mostrarla
plt.savefig('/content/drive/MyDrive/datasets_colab/imagenes/wordArgDark1.jpg', bbox_inches='tight', dpi=75)

# Muestra la figura después de guardarla
plt.show()
 ```

![WordCloud Argentina Dark](https://github.com/martinfernandoortiz/wordCloud_Geografia/blob/main/imagenes/wordArgDark1.jpg "WordCloud Argentina Dark")

### WordCloud Dark
 ```python
wordcloud = WordCloud(width=800, height=400, relative_scaling=0.5,
                      background_color="black",
               max_font_size = 42, min_font_size = 6,
               colormap = "tab20c",
               #max_words = 3,
               #contour_width = .1, contour_color = "grey",
               stopwords = stopwords_total).generate(text)

# Muestra la WordCloud con matplotlib
plt.figure(figsize=(10, 5))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')

plt.title("Nube Dark", fontsize=16, color="#ff7f0e")

# Guarda la figura antes de mostrarla
plt.savefig('/content/drive/MyDrive/datasets_colab/imagenes/wordDark.jpg', bbox_inches='tight', dpi=75)

# Muestra la figura después de guardarla
plt.show()
 ```

![WordCloud Dark](https://github.com/martinfernandoortiz/wordCloud_Geografia/blob/main/imagenes/wordDark.jpg "WordCloud Dark")

### Palabras más frecuentes

 ```python

# Configura la WordCloud con el tamaño deseado y fondo oscuro
wordcloud = WordCloud(width=800, height=400, background_color='black', colormap='Paired', stopwords=stopwords_total)

# Genera las frecuencias de palabras
word_frequencies = wordcloud.process_text(text)

# Crea un DataFrame con las frecuencias de palabras
df_word_frequencies = pd.DataFrame(list(word_frequencies.items()), columns=['Word', 'Frecuencia'])

# Ordena el DataFrame de forma descendente por frecuencia
df_word_frequencies = df_word_frequencies.sort_values(by='Frecuencia', ascending=False)

# Toma solo las 30 palabras más repetidas
df_top_30 = df_word_frequencies.head(30)

# Obtén colores de la WordCloud utilizando el modelo de color HSL
hsl_colors = [(i / len(df_top_30), 0.5, 0.5) for i in range(len(df_top_30))]

# Convierte colores HSL a RGB
rgb_colors = [colorsys.hls_to_rgb(h, l, s) for (h, l, s) in hsl_colors]

# Convierte valores RGB a formato hexadecimal
hex_colors = ['#%02x%02x%02x' % (int(r * 255), int(g * 255), int(b * 255)) for (r, g, b) in rgb_colors]

# Crea un gráfico de barras con Matplotlib
fig, ax = plt.subplots(figsize=(10, 5), facecolor='black')  # Fondo negro de la figura
bars = ax.bar(df_top_30['Word'], df_top_30['Frecuencia'], color=hex_colors)

# Agrega etiquetas con el número en cada barra sin decimales
for bar, label in zip(bars, df_top_30['Frecuencia']):
    height = bar.get_height()
    ax.text(bar.get_x() + bar.get_width() / 2, height, f'{int(label)}', ha='center', va='bottom', color='#b2df8a')

# Ajusta el fondo y el color del texto y etiquetas
ax.set_facecolor('black')
ax.tick_params(axis='x', colors='#b2df8a')
ax.tick_params(axis='y', colors='#fdbf6f')
ax.spines['bottom'].set_color('#a6cee3')
ax.spines['top'].set_color('black')
ax.spines['right'].set_color('black')
ax.spines['left'].set_color('#cab2d6')

# Etiquetas del eje x rotadas -90 grados
ax.set_xticklabels(df_top_30['Word'], rotation=90, ha='center', color='#fb9a99')

# Etiqueta del eje y rotada 90 grados
ax.set_ylabel('Frecuencia', rotation=90, va='center', color='#fdbf6f')

plt.title("Frecuencia de Palabras", fontsize=16, color="#fdbf6f")

# Guarda la figura antes de mostrarla
plt.savefig('/content/drive/MyDrive/datasets_colab/imagenes/barras.jpg', bbox_inches='tight', dpi=75)

# Muestra la figura después de guardarla
plt.show()
 ```

![Barras](https://github.com/martinfernandoortiz/wordCloud_Geografia/blob/main/imagenes/barras.jpg "Barras")
