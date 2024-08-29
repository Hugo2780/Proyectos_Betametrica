MODULO II: Análisis Exploratorio
================
Hugo Donoso
2024-08-21

``` r
datos<- read.xlsx("E:/Betametrica/Modulo II/Base.xlsx", sheet = "Hoja1", detectDates = T )
datos$Trimestres <- seq(as.Date("2000/03/01"),
                        as.Date("2024/03/01"), 
                        by="quarter")
```

Graficos con ggplot

``` r
ggplot(data=datos)+
        aes(x=Trimestres)+
        aes(y=`Manufactura.de.productos.no.alimenticios`)+
        geom_line(color="red", alpha=0.5, linewidth=1)+
         geom_point(color="green",alpha=0.3, size=2)+
         geom_hline(yintercept = mean(datos$`Manufactura.de.productos.no.alimenticios`,na.rm = TRUE),
                    col="purple")
```

![](MODULO_2_Hugo_Donoso_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

boxplot usando ggplot2

``` r
ggplot(data = datos) +
  aes(x = "", y = `Manufactura.de.productos.no.alimenticios`) +  # Ajusta el nombre de la columna según tus datos
  geom_boxplot(fill = "skyblue") +
  labs(title = "Boxplot de Manufacturas", y = "Manufacturas (en millones)") +
  theme_minimal()
```

![](MODULO_2_Hugo_Donoso_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

GRÁFICO DE BARRAS NO APILADAS PARA 2 VARIABLES

Crear un dataframe con las dos variables

``` r
datos_barras <- datos %>%
  select(Trimestres, Refinados_de_petroleo = Refinados.de.petroleo, Comercio = `Comercio`)

datos_barras_long <- melt(datos_barras, id.vars = "Trimestres")
```

# Crear el gráfico de barras

``` r
ggplot(data = datos_barras_long) +
  aes(x = Trimestres, y = value, fill = variable) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Refinados de petroleo vs Comercio",
       x = "Trimestres",
       y = "Millones de USD",
       fill = "Variable") +
  theme_minimal()
```

![](MODULO_2_Hugo_Donoso_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

GRAFICO DE LINEAS CON FACET

SE Escoge dos variables: Comercio y Explotación de minas y canteras

``` r
ggplot(data = datos_barras_long) +
  aes(x = Trimestres, y = value, color = variable) +
  geom_line(linewidth = 1) +
  geom_smooth(method = "lm", se = FALSE, linetype = "dashed") +  # Añadir línea de tendencia
  facet_wrap(~ variable, scales = "free_y") +
  labs(title = "Comercio vs Explotacion de minas y canteras",
       x = "Trimestres",
       y = "Millones de USD") +
  theme_minimal()
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](MODULO_2_Hugo_Donoso_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Gráfico usando dygraphs o plotly

``` r
p <- ggplot(data = datos) +
  aes(x = Trimestres, y = `Comercio`) +
  geom_line(color = "orange", alpha = 0.5, linewidth = 1) +
  geom_point(color = "blue", alpha = 0.3, size = 2) +
  geom_hline(yintercept = mean(datos$`Comercio`, na.rm = TRUE),
             col = "grey")


interactive_plot <- ggplotly(p)

ggplotly(p)
```

<div class="plotly html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-1d85fac42875110be148" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-1d85fac42875110be148">{"x":{"data":[{"x":[11017,11109,11201,11292,11382,11474,11566,11657,11747,11839,11931,12022,12112,12204,12296,12387,12478,12570,12662,12753,12843,12935,13027,13118,13208,13300,13392,13483,13573,13665,13757,13848,13939,14031,14123,14214,14304,14396,14488,14579,14669,14761,14853,14944,15034,15126,15218,15309,15400,15492,15584,15675,15765,15857,15949,16040,16130,16222,16314,16405,16495,16587,16679,16770,16861,16953,17045,17136,17226,17318,17410,17501,17591,17683,17775,17866,17956,18048,18140,18231,18322,18414,18506,18597,18687,18779,18871,18962,19052,19144,19236,19327,19417,19509,19601,19692,19783],"y":[597859.04607693595,654456.98135304602,721966.59605116502,736874.324411751,885445.66409305797,888402.86730448599,893275.74507279706,922498.52730909002,908748.67116619204,936588.94608542998,936974.66681764205,937459.17078012903,989891.17772632302,974342.88148232806,985226.13078304299,1002537.5248533899,992314.88980320701,1022889.7468617799,1050948.46786972,1077894.30385557,1132936.4507506201,1157745.0396688001,1158950.3428507801,1212753.8450088601,1230857.56148665,1256640.72489596,1282933.9098034899,1280103.0594873601,1280289.25406183,1285025.3119027901,1341404.7002612001,1454865.7998506301,1596562.76929815,1735431.59830658,1860970.1360877,1809749.87926985,1736936.52242592,1719153.3214402299,1754868.8524072999,1817929.4931048299,1902760.2170696401,1981970.94897333,2062002.9346268401,2147666.0050074402,2287481.7878958099,2396170.91381831,2481103.8768650298,2576756.83039233,2644103.6092694299,2704586.7760557402,2748632.3190509002,2795993.5532102999,2989784.31102882,3055742.6913779601,3163439.8625829602,3244629.2402410801,3308610.28015737,3385186.72060426,3499508.4612524398,3462708.0542613701,3482807.12237775,3438336.90146446,3410041.4463350698,3397826.1397694498,3210087.5138594098,3337168.3799733901,3381448.38844166,3497470.1465373598,3577847.76991498,3631273.9227296999,3597984.4530052198,3596814.3726509102,3951708.1509743901,3854142.68502616,3743401.0520483302,3699622.1119511798,3739000.6260189102,3829329.1164277499,3872594.0195836602,3835762.2379713301,3753050.64932627,2993465.3763209502,3299283.4129672302,3556615.5613876302,3654158.1086917599,3990411.9091044501,4169258.3586398,4400528.6235628398,4651384.18324262,4591801.9557489399,4958755.3996510999,4696958.4613573505,4905219.1490892703,4863496.6132159997,4693909.1267459802,4619885.34824319,4804580.7535879603],"text":["Trimestres: 2000-03-01<br />Comercio:  597859.0","Trimestres: 2000-06-01<br />Comercio:  654457.0","Trimestres: 2000-09-01<br />Comercio:  721966.6","Trimestres: 2000-12-01<br />Comercio:  736874.3","Trimestres: 2001-03-01<br />Comercio:  885445.7","Trimestres: 2001-06-01<br />Comercio:  888402.9","Trimestres: 2001-09-01<br />Comercio:  893275.7","Trimestres: 2001-12-01<br />Comercio:  922498.5","Trimestres: 2002-03-01<br />Comercio:  908748.7","Trimestres: 2002-06-01<br />Comercio:  936588.9","Trimestres: 2002-09-01<br />Comercio:  936974.7","Trimestres: 2002-12-01<br />Comercio:  937459.2","Trimestres: 2003-03-01<br />Comercio:  989891.2","Trimestres: 2003-06-01<br />Comercio:  974342.9","Trimestres: 2003-09-01<br />Comercio:  985226.1","Trimestres: 2003-12-01<br />Comercio: 1002537.5","Trimestres: 2004-03-01<br />Comercio:  992314.9","Trimestres: 2004-06-01<br />Comercio: 1022889.7","Trimestres: 2004-09-01<br />Comercio: 1050948.5","Trimestres: 2004-12-01<br />Comercio: 1077894.3","Trimestres: 2005-03-01<br />Comercio: 1132936.5","Trimestres: 2005-06-01<br />Comercio: 1157745.0","Trimestres: 2005-09-01<br />Comercio: 1158950.3","Trimestres: 2005-12-01<br />Comercio: 1212753.8","Trimestres: 2006-03-01<br />Comercio: 1230857.6","Trimestres: 2006-06-01<br />Comercio: 1256640.7","Trimestres: 2006-09-01<br />Comercio: 1282933.9","Trimestres: 2006-12-01<br />Comercio: 1280103.1","Trimestres: 2007-03-01<br />Comercio: 1280289.3","Trimestres: 2007-06-01<br />Comercio: 1285025.3","Trimestres: 2007-09-01<br />Comercio: 1341404.7","Trimestres: 2007-12-01<br />Comercio: 1454865.8","Trimestres: 2008-03-01<br />Comercio: 1596562.8","Trimestres: 2008-06-01<br />Comercio: 1735431.6","Trimestres: 2008-09-01<br />Comercio: 1860970.1","Trimestres: 2008-12-01<br />Comercio: 1809749.9","Trimestres: 2009-03-01<br />Comercio: 1736936.5","Trimestres: 2009-06-01<br />Comercio: 1719153.3","Trimestres: 2009-09-01<br />Comercio: 1754868.9","Trimestres: 2009-12-01<br />Comercio: 1817929.5","Trimestres: 2010-03-01<br />Comercio: 1902760.2","Trimestres: 2010-06-01<br />Comercio: 1981970.9","Trimestres: 2010-09-01<br />Comercio: 2062002.9","Trimestres: 2010-12-01<br />Comercio: 2147666.0","Trimestres: 2011-03-01<br />Comercio: 2287481.8","Trimestres: 2011-06-01<br />Comercio: 2396170.9","Trimestres: 2011-09-01<br />Comercio: 2481103.9","Trimestres: 2011-12-01<br />Comercio: 2576756.8","Trimestres: 2012-03-01<br />Comercio: 2644103.6","Trimestres: 2012-06-01<br />Comercio: 2704586.8","Trimestres: 2012-09-01<br />Comercio: 2748632.3","Trimestres: 2012-12-01<br />Comercio: 2795993.6","Trimestres: 2013-03-01<br />Comercio: 2989784.3","Trimestres: 2013-06-01<br />Comercio: 3055742.7","Trimestres: 2013-09-01<br />Comercio: 3163439.9","Trimestres: 2013-12-01<br />Comercio: 3244629.2","Trimestres: 2014-03-01<br />Comercio: 3308610.3","Trimestres: 2014-06-01<br />Comercio: 3385186.7","Trimestres: 2014-09-01<br />Comercio: 3499508.5","Trimestres: 2014-12-01<br />Comercio: 3462708.1","Trimestres: 2015-03-01<br />Comercio: 3482807.1","Trimestres: 2015-06-01<br />Comercio: 3438336.9","Trimestres: 2015-09-01<br />Comercio: 3410041.4","Trimestres: 2015-12-01<br />Comercio: 3397826.1","Trimestres: 2016-03-01<br />Comercio: 3210087.5","Trimestres: 2016-06-01<br />Comercio: 3337168.4","Trimestres: 2016-09-01<br />Comercio: 3381448.4","Trimestres: 2016-12-01<br />Comercio: 3497470.1","Trimestres: 2017-03-01<br />Comercio: 3577847.8","Trimestres: 2017-06-01<br />Comercio: 3631273.9","Trimestres: 2017-09-01<br />Comercio: 3597984.5","Trimestres: 2017-12-01<br />Comercio: 3596814.4","Trimestres: 2018-03-01<br />Comercio: 3951708.2","Trimestres: 2018-06-01<br />Comercio: 3854142.7","Trimestres: 2018-09-01<br />Comercio: 3743401.1","Trimestres: 2018-12-01<br />Comercio: 3699622.1","Trimestres: 2019-03-01<br />Comercio: 3739000.6","Trimestres: 2019-06-01<br />Comercio: 3829329.1","Trimestres: 2019-09-01<br />Comercio: 3872594.0","Trimestres: 2019-12-01<br />Comercio: 3835762.2","Trimestres: 2020-03-01<br />Comercio: 3753050.6","Trimestres: 2020-06-01<br />Comercio: 2993465.4","Trimestres: 2020-09-01<br />Comercio: 3299283.4","Trimestres: 2020-12-01<br />Comercio: 3556615.6","Trimestres: 2021-03-01<br />Comercio: 3654158.1","Trimestres: 2021-06-01<br />Comercio: 3990411.9","Trimestres: 2021-09-01<br />Comercio: 4169258.4","Trimestres: 2021-12-01<br />Comercio: 4400528.6","Trimestres: 2022-03-01<br />Comercio: 4651384.2","Trimestres: 2022-06-01<br />Comercio: 4591802.0","Trimestres: 2022-09-01<br />Comercio: 4958755.4","Trimestres: 2022-12-01<br />Comercio: 4696958.5","Trimestres: 2023-03-01<br />Comercio: 4905219.1","Trimestres: 2023-06-01<br />Comercio: 4863496.6","Trimestres: 2023-09-01<br />Comercio: 4693909.1","Trimestres: 2023-12-01<br />Comercio: 4619885.3","Trimestres: 2024-03-01<br />Comercio: 4804580.8"],"type":"scatter","mode":"lines+markers","line":{"width":3.7795275590551185,"color":"rgba(255,165,0,0.5)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","marker":{"autocolorscale":false,"color":"rgba(0,0,255,1)","opacity":0.29999999999999999,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(0,0,255,1)"}},"frame":null},{"x":[10578.700000000001,20221.299999999999],"y":[2543803.8540599216,2543803.8540599216],"text":"yintercept: 2543804","type":"scatter","mode":"lines","line":{"width":1.8897637795275593,"color":"rgba(190,190,190,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":26.228310502283104,"r":7.3059360730593621,"b":40.182648401826491,"l":54.794520547945211},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[10578.700000000001,20221.299999999999],"tickmode":"array","ticktext":["2000","2010","2020"],"tickvals":[10957,14610,18262],"categoryorder":"array","categoryarray":["2000","2010","2020"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"y","title":{"text":"Trimestres","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[379814.22839822771,5176800.2173298085],"tickmode":"array","ticktext":["1e+06","2e+06","3e+06","4e+06","5e+06"],"tickvals":[1000000,2000000,3000000,4000000,5000000],"categoryorder":"array","categoryarray":["1e+06","2e+06","3e+06","4e+06","5e+06"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"x","title":{"text":"Comercio","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"12dc1859538b":{"x":{},"y":{},"type":"scatter"},"12dc77501360":{"x":{},"y":{}},"12dc52c51cc9":{"yintercept":{}}},"cur_data":"12dc1859538b","visdat":{"12dc1859538b":["function (y) ","x"],"12dc77501360":["function (y) ","x"],"12dc52c51cc9":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
