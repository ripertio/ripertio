---
title: Die skandinavischen Länder sind die nachhaltigsten ... oder doch nicht!
summary: Bei einem Blick auf die Nachhaltigkeits-Rankings könnte man meinen, dass vor allem skandinavische und europäische Länder in Sachen ökologischer Nachhaltigkeit die Nase vorn haben. Schaut man sich jedoch das Modell der planetaren Grenzen an, sieht die Sache schon anders aus; Diese Länder verbrauchen deutlich mehr, als die Erde regenerieren kann, währenddessen Länder des globalen Südens häufig innerhalb der ökologischen Belastungsgrenze bleiben
date: 2024-01-28
draft: false
tags: []
categories:
    - helicopter-sustainability
---
{{<alert "notiz">}}
## TLDR
- Bei einem Blick auf die Nachhaltigkeits-Rankings könnte man meinen, dass vor allem skandinavische und europäische Länder in Sachen ökologischer Nachhaltigkeit die Nase vorn haben.
- Schaut man sich jedoch das Modell der planetaren Grenzen an, sieht die Sache schon anders aus: Diese Länder verbrauchen deutlich mehr, als die Erde regenerieren kann, währenddessen Länder des globalen Südens häufig innerhalb der ökologischen Belastungsgrenze bleiben
{{</alert>}}

Wenn man wissen will, welche Länder am nachhaltigsten sind, landet man schnell bei verschiedenen Rankings, die Skandinavien ganz oben sehen. Laut dem Environmental Performance Index (EPI) 2022 sind Dänemark, Großbritannien, Finnland, Malta, Schweden und Luxemburg besonders nachhaltig [1](https://de.statista.com/statistik/daten/studie/983991/umfrage/ranking-der-umweltfreundlichsten-laender-nach-dem-environmental-performance-index/). Norwegen wurde auch als das nachhaltigste Land der Welt bezeichnet, gefolgt von Schweden, Finnland und der Schweiz[3](https://www.umweltdialog.de/de/management/ratings-rankings/2018/Norwegen-verdraengt-Schweden-von-Platz-1-im-Nachhaltigkeitsranking-der-Laender.php). Der EPI berücksichtigt verschiedene umweltrelevante Faktoren wie Klimawandel, Biodiversität, Luftqualität, Wasserverschmutzung, Ökosystemleistung und vieles mehr.

{{<centerimg alt="" src="./EPI_2022_countries.webp" maxwidth="700px">}}

Diese Bewertung deckte sich ziemlich mit meiner bisherigen Vorstellung darüber, welche Länder nachhaltiger sind und welche weniger. Doch dann stieß ich auf dieses Paper: [The social shortfall and ecological overshoot of nations](https://www.nature.com/articles/s41893-021-00799-z) von Fanning et al. 2021. Sie präsentieren eine etwas andere Analyse.

## Der Norden Verbraucht Mehr Als Der Süden
Was haben Fanning et al. in ihrem Paper gemacht? Sie haben die Auswirkungen auf verschiedene Erdsysteme von 1990 bis 2015 ausgewertet, basierend auf verschiedenen Datenbanken. Mit Hilfe einer abgewandelten Variante des Modells der planetaren Grenzen und berechneten Schwellenwerten haben sie für jedes Land den Overshoot, also den Überverbrauch, berechnet. Die Schwellenwerte für die Länder wurden auf der Grundlage von alternativen Ansichten zur Verteilungsgerechtigkeit berechnet.

Das Modell der planetaren Grenzen wurde erstmals 2009 von Johan Rockström und einer Gruppe von 28 international anerkannten Wissenschaftlern vorgestellt. Aktuelle Untersuchungen aus dem Jahr 2023 nehmen mittlerweile alle neun Erdsysteme unter die Lupe und kommen zu dem Ergebnis, dass sechs davon überbeansprucht werden.
Die Schwellenwerte für die einzelnen Länder wurden auf Basis alternativer Sichtweisen zur Verteilungsgerechtigkeit berechnet. Wer sich dafür näher interessiert, kann das hier nachlesen: [Allocating planetary boundaries to large economies: Distributional consequences of alternative perspectives on distribitive fairness - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0959378019305679?via%3Dihub).

Die folgenden drei interaktiven Weltkarten zeigen den Überverbrauch (Overshoot) der Länder in Bezug auf CO2-Emissionen, Landnutzungsänderungen und Materialverbrauch. Je dunkler die Farbe, desto stärker wird das jeweilige Erdsystem überbeansprucht.

Für alle drei Karten habe ich die Daten verwendet, die Fanning et al. in ihrem Paper verwendet haben(https://goodlife.leeds.ac.uk/). Den Code für die Erstellung dieser Karten findet ihr in diesem [GitHub-Repository](https://github.com/ripertio/Choropleth-Map-Ecologicla-Overshoot-Social-Shortfall-Data-Fanning2021-) 😆 (war eine cool Herausforderung diese Karte so zu bauen → Feedback/Verbesserungsvorschläge sind gerne willkommen). Tipp: Ihr könnt alle drei interaktiven Karten in einem neuen Tab öffnen und euch in groß anschauen. Über die einzelnen Länder zu schweben und den Slider zu verschieben, ist ziemlich cool - Feedback und Verbesserungsvorschläge sind jederzeit willkommen!

### Überschreitung an CO2-Emissionen

Die erste animierte Weltkarte zeigt die Überschreitung (Overshoot) der Länder in Bezug auf die CO2-Emissionen. Je dunkler die Farbe, desto stärker belasten die Länder das Erdsystem und die Zahlen geben an, um das wievielfache das jeweilige Land das Erdsystem überbeansprucht. Wenn alle Zahlen unter oder gleich 1 liegen, bedeutet dies, dass das Land sich im sicheren Bereich befindet, während Zahlen größer als 1 außerhalb der planetaren Grenzen liegen. Eine Zahl von 3 bedeutet also, dass das Land  drei Erden benötigen würde um innerhalb  der Belastungsgrenze zu bleiben.

{{<centerimg alt="GIF-emission" src="./feature_CO2_Emissions_animated.webp" maxwidth="100%">}}

Hier das gleiche als interactives html: <a href="CO2 Emissions_interactive.html">Interactive CO2 Emissions Karte</a>

### Überschreitung Der Landnutzungsgrenzen

Die zweite Karte zeigt den "Land Use Change Overshoot". Dieser Begriff bezieht sich auf die Überschreitung der ökologischen Grenzen durch die Veränderung der Landnutzung. Das bedeutet, dass die menschliche Aktivität die nachhaltige und regenerierbare Nutzung von Land übersteigt, was negative Auswirkungen auf die Umwelt haben kann. Klassische Beispiele sind die Auslaugung der Böden oder die Versiegelung der Böden durch Gebäude und andere Infrastrukturen.

{{<centerimg alt="GIF_land change" src="./Land-System_Change_animated.webp" maxwidth="100%">}}

Hier das gleiche als interactives html: <a href="CO2 Land-System Change_interactive.html">Interactive Landnutzungsgrenzen Karte</a>

### Überbeanspruchung Des Material-Fußabdrucks

Die dritte Karte zeigt den Material-Fußabdruck der einzelnen Länder. Der "Material Footprint" bezieht sich auf die Gesamtmenge an Rohstoffen, die zur Deckung des Verbrauchs abgebaut wird. Dazu gehören Metalle, Biomasse, fossile Brennstoffe und nichtmetallische Mineralien. Ein Überschuss beim Material-Fußabdruck bedeutet, dass wir Ressourcen schneller abbauen und verbrauchen, als die Erde sie regenerieren kann. Dies führt zu Ressourcenverknappung, Zerstörung von Lebensräumen und Verlust der Artenvielfalt.

{{<centerimg alt="GIF_Footprint" src="./Material_Footprint_animated.webp" maxwidth="100%">}}

Hier das gleiche als interactives html: <a href="Material Footprint_interactive.html">Interactive Material-Fußabdruck Karte</a>


## Und Jetzt?
- Meine ursprüngliche Annahme war, dass vor allem skandinavische Länder besonders ökologisch nachhaltig sind. Auch die Top 20 des Environmental Performance Index sind durchweg Länder des globalen Nordens.
- Die Analyse von Fanning et al. zeichnet jedoch ein ganz anderes Bild: Vergleicht man die einzelnen Länder mit den thematischen Karten, stellt man fest, dass die vom EPI als besonders nachhaltig eingestuften Länder weit über den Belastungsgrenzen liegen. Viele Länder des globalen Südens hingegen bleiben innerhalb dieser Grenzen.
- Ich denke, wir sollten unsere Sichtweise auf skandinavische und andere Länder des globalen Nordens, die viel für den Umweltschutz und sauberes Wasser tun, überdenken. Wir sollten sie nicht als einzige Indikatoren für ökologische Nachhaltigkeit betrachten. Genau für solche Analysen-Zwecke wurden Modelle wie die "Planetary Boundaries" und "Ecological Ceilings" entwickelt. Lasst uns diese als Maßstab verwenden!


