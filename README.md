# cityEuropee_piu_vicine

Creare una mappa delle capitali europee più vicine ai comuni italiani usando QGIS.

------------- IN COSTRUZIONE ---------- 

<!-- TOC -->

- [cityEuropee\_piu\_vicine](#cityeuropee_piu_vicine)
- [INTRO](#intro)
- [FLOWCHART](#flowchart)
  - [tematizzare i punti usando l'attributo Capital](#tematizzare-i-punti-usando-lattributo-capital)
  - [poligoni di voronoi](#poligoni-di-voronoi)
  - [tematizzare i poligoni di voronoi con lo stesso tema](#tematizzare-i-poligoni-di-voronoi-con-lo-stesso-tema)
    - [espressione utilizzata](#espressione-utilizzata)
  - [assegnare comune](#assegnare-comune)
  - [Campi virtuali](#campi-virtuali)
- [RIFERIMENTI](#riferimenti)
- [DISCLAIMER](#disclaimer)

<!-- /TOC -->

# INTRO

Creare una mappa delle capitali europee più vicine ai comuni italiani usando QGIS e tematizzando i comuni in funzione del tema del punto che caratterizza la capitale europea.

![](imgs/img01.png)

# FLOWCHART

## tematizzare i punti usando l'attributo Capital

![](imgs/img02.png)

## poligoni di voronoi

![](imgs/img03.png)

## tematizzare i poligoni di voronoi con lo stesso tema

![](imgs/img04.png)

### espressione utilizzata

```
overlay_contains('capitali_europee',get_symbol_colors())[0]
```

![](imgs/img05.png)

**NB:** la funzione `get_symbol_colors()` viene installata in QGIS dopo aver installato il plugin DataPloty, lo ritroveremo nel field calc sotto il gruppo `DataPloty`.

## assegnare comune

I poligoni di voronoi intersecano alcuni comuni lungo i perimetri di delimitazione, a chi assegnare il comune?

![](imgs/img06.png)

Potremmo usare vari criteri:
1. dove ricade il centroide del comune rispetto al poligono di voronoi;
2. punto che individuo il municipio;
3. confine condiviso pù lungo;
4. porzione di superficie maggiore;
5. tagliare il comune e assegnare il colore in funzione a dove ricade;
6. ecc...

per facilità, lascio decidere a QGIS usando la seguente espressione (con predicato _intersects_):

```
overlay_intersects(
layer:='voronoi',
expression:= "colore" )[0]
```

ricordo che l'attributo `colore` del layer `voronoi` è un campo virtuale popolato con l'espressione `overlay_contains('capitali_europee',get_symbol_colors())[0]`:

![](imgs/img07.png)

## Campi virtuali

Avendo utilizzato i campi virtuali ora sarà semplice ed immediato cambiare il tema ai punti e di conseguenza cambieranno i colori dei comuni. È una operazione un po' pesante e dipende molto dalle risorse del PC. (PURTROPPO CAMBIANDO I TEMI DEI PUNTI QGIS CRASHA, QUI ISSUE)

# RIFERIMENTI

- <https://puntofisso.net/blog/posts/qgis-closest-capital/>
- QGIS: https://www.qgis.org/it/site/


# DISCLAIMER

Il presente contenuto è stato realizzato da _**Salvatore Fiandaca**_ nel mese di agosto 2023 utilizzando [QGIS 3.28 Firenze LTR](https://qgis.org/it/site/) e distribuito con licenza [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.it)