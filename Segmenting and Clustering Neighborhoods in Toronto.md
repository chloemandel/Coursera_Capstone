# Importing required libraries 


```python
import requests # library to handle requests
import pandas as pd # library for data analysis
import numpy as np # library to handle data in a vectorized manner
import random # library for random number generation

#!conda install -c conda-forge geopy --yes 
from geopy.geocoders import Nominatim # convert an address into latitude 

from IPython.display import Image 
from IPython.core.display import HTML 
from IPython.display import display_html

from pandas.io.json import json_normalize
from bs4 import BeautifulSoup
!conda install -c conda-forge folium=0.5.0 --yes 
import folium # plotting library

from sklearn.cluster import KMeans # import KMeans from clustering stage

# Matplotlib and associated plotting modules
import matplotlib.cm as cm
import matplotlib.colors as colors
```

    Collecting package metadata (current_repodata.json): done
    Solving environment: done
    
    # All requested packages already installed.
    


# Scraping the Toronto data Wikipedia page 

Creates a Beautiful Soup object and scrapes data table from the Wiki page.


```python
url = 'https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M'
data = requests.get(url).text
soup = BeautifulSoup(data, 'html5lib')
table = str(soup.find('table'))
from IPython.display import display_html
display_html(table, raw = True)
```


<table cellpadding="2" cellspacing="0" rules="all" style="width:100%; border-collapse:collapse; border:1px solid #ccc;">

<tbody><tr>
<td style="width:11%; vertical-align:top; color:#ccc;">
<p><b>M1A</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="width:11%; vertical-align:top; color:#ccc;">
<p><b>M2A</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="width:11%; vertical-align:top;">
<p><b>M3A</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Parkwoods" title="Parkwoods">Parkwoods</a>)</span>
</p>
</td>
<td style="width:11%; vertical-align:top;">
<p><b>M4A</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Victoria_Village" title="Victoria Village">Victoria Village</a>)</span>
</p>
</td>
<td style="width:11%; vertical-align:top;">
<p><b>M5A</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/Regent_Park" title="Regent Park">Regent Park</a> / <a href="/wiki/Harbourfront,_Toronto" title="Harbourfront, Toronto">Harbourfront</a>)</span>
</p>
</td>
<td style="width:11%; vertical-align:top;">
<p><b>M6A</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Lawrence_Manor" title="Lawrence Manor">Lawrence Manor</a> / <a href="/wiki/Lawrence_Heights" title="Lawrence Heights">Lawrence Heights</a>)</span>
</p>
</td>
<td style="width:11%; vertical-align:top;">
<p><b>M7A</b><br/><span style="font-size:85%;"><a href="/wiki/Queen%27s_Park_(Toronto)" title="Queen's Park (Toronto)">Queen's Park</a><br/>(Ontario Provincial Government)</span>
</p>
</td>
<td style="width:11%; vertical-align:top; color:#ccc;">
<p><b>M8A</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="width:11%; vertical-align:top;">
<p><b>M9A</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a class="mw-redirect" href="/wiki/Islington_Avenue" title="Islington Avenue">Islington Avenue</a>)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1B</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Malvern,_Toronto" title="Malvern, Toronto">Malvern</a> / <a href="/wiki/Rouge,_Toronto" title="Rouge, Toronto">Rouge</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2B</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3B</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Don_Mills" title="Don Mills">Don Mills</a>)<br/>North</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4B</b><br/><span style="font-size:85%;"><a href="/wiki/East_York" title="East York">East York</a><br/>(<a class="mw-redirect" href="/wiki/Parkview_Hill" title="Parkview Hill">Parkview Hill</a> / <a class="mw-redirect" href="/wiki/Woodbine_Gardens" title="Woodbine Gardens">Woodbine Gardens</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5B</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/Garden_District,_Toronto" title="Garden District, Toronto">Garden District</a>, <a href="/wiki/Ryerson_University" title="Ryerson University">Ryerson</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6B</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(Glencairn)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7B</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8B</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9B</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a class="mw-redirect" href="/wiki/West_Deane_Park" title="West Deane Park">West Deane Park</a> / <a href="/wiki/Princess_Gardens" title="Princess Gardens">Princess Gardens</a> / <a class="mw-redirect" href="/wiki/Martin_Grove_Road" title="Martin Grove Road">Martin Grove</a> / <a class="mw-redirect" href="/wiki/Islington,_Toronto" title="Islington, Toronto">Islington</a> / Cloverdale)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1C</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a class="mw-redirect" href="/wiki/Rouge_Hill" title="Rouge Hill">Rouge Hill</a> / <a class="mw-redirect" href="/wiki/Port_Union,_Ontario" title="Port Union, Ontario">Port Union</a> / <a href="/wiki/Highland_Creek,_Toronto" title="Highland Creek, Toronto">Highland Creek</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2C</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3C</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Don_Mills" title="Don Mills">Don Mills</a>)<br/>South<br/>(<a href="/wiki/Flemingdon_Park" title="Flemingdon Park">Flemingdon Park</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4C</b><br/><span style="font-size:85%;"><a href="/wiki/East_York" title="East York">East York</a><br/>(<a class="mw-redirect" href="/wiki/Woodbine_Heights" title="Woodbine Heights">Woodbine Heights</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5C</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/St._James_Town" title="St. James Town">St. James Town</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6C</b><br/><span style="font-size:85%;"><a href="/wiki/York,_Toronto" title="York, Toronto">York</a><br/>(<a class="mw-redirect" href="/wiki/Humewood-Cedarvale" title="Humewood-Cedarvale">Humewood-Cedarvale</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7C</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8C</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9C</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(Eringate / Bloordale Gardens / Old Burnhamthorpe / <a href="/wiki/Markland_Wood" title="Markland Wood">Markland Wood</a>)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1E</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Guildwood" title="Guildwood">Guildwood</a> / <a href="/wiki/Morningside,_Toronto" title="Morningside, Toronto">Morningside</a> / <a href="/wiki/West_Hill,_Toronto" title="West Hill, Toronto">West Hill</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2E</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3E</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4E</b><br/><span style="font-size:85%;">East Toronto<br/>(<a href="/wiki/The_Beaches" title="The Beaches">The Beaches</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5E</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(Berczy Park)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6E</b><br/><span style="font-size:85%;"><a href="/wiki/York,_Toronto" title="York, Toronto">York</a><br/>(<a class="mw-redirect" href="/wiki/Caledonia-Fairbanks" title="Caledonia-Fairbanks">Caledonia-Fairbanks</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7E</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8E</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9E</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1G</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Woburn,_Toronto" title="Woburn, Toronto">Woburn</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2G</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3G</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4G</b><br/><span style="font-size:85%;"><a href="/wiki/East_York" title="East York">East York</a><br/>(<a href="/wiki/Leaside" title="Leaside">Leaside</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5G</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(Central <a href="/wiki/Bay_Street" title="Bay Street">Bay Street</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6G</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(Christie)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7G</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8G</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9G</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1H</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Woburn,_Toronto" title="Woburn, Toronto">Cedarbrae</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2H</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Hillcrest_Village" title="Hillcrest Village">Hillcrest Village</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3H</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Bathurst_Manor" title="Bathurst Manor">Bathurst Manor</a> / <a class="mw-redirect" href="/wiki/Wilson_Heights,_Toronto" title="Wilson Heights, Toronto">Wilson Heights</a> / <a href="/wiki/Downsview" title="Downsview">Downsview</a> North)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4H</b><br/><span style="font-size:85%;"><a href="/wiki/East_York" title="East York">East York</a><br/>(<a href="/wiki/Thorncliffe_Park" title="Thorncliffe Park">Thorncliffe Park</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5H</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a class="mw-redirect" href="/wiki/Richmond_Street" title="Richmond Street">Richmond</a> / Adelaide / <a href="/wiki/King_Street_(Toronto)" title="King Street (Toronto)">King</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6H</b><br/><span style="font-size:85%;">West Toronto<br/>(<a href="/wiki/Dufferin_Street" title="Dufferin Street">Dufferin</a> / <a class="mw-redirect" href="/wiki/Dovercourt_Village" title="Dovercourt Village">Dovercourt Village</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7H</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8H</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9H</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1J</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Scarborough_Village" title="Scarborough Village">Scarborough Village</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2J</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(Fairview / <a href="/wiki/Henry_Farm" title="Henry Farm">Henry Farm</a> / Oriole)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3J</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/York_University_Heights" title="York University Heights">Northwood Park</a> / <a href="/wiki/York_University" title="York University">York University</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4J</b><br/><span style="font-size:85%;"><a href="/wiki/East_York" title="East York">East York</a><br/>East Toronto<br/>(<a class="mw-redirect" href="/wiki/The_Danforth" title="The Danforth">The Danforth</a> <br/> East)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5J</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a class="mw-redirect" href="/wiki/Harbourfront_(Toronto)" title="Harbourfront (Toronto)">Harbourfront</a> East / <a href="/wiki/Union_Station_(Toronto)" title="Union Station (Toronto)">Union Station</a> / <a href="/wiki/Toronto_Islands" title="Toronto Islands">Toronto Islands</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6J</b><br/><span style="font-size:85%;">West Toronto<br/>(<a href="/wiki/Little_Portugal,_Toronto" title="Little Portugal, Toronto">Little Portugal</a> / <a class="mw-redirect" href="/wiki/Trinity%E2%80%93Bellwoods" title="Trinity–Bellwoods">Trinity</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7J</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8J</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9J</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1K</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a class="mw-redirect" href="/wiki/Kennedy_Park,_Toronto" title="Kennedy Park, Toronto">Kennedy Park</a> / <a href="/wiki/Ionview" title="Ionview">Ionview</a> / East <a class="mw-redirect" href="/wiki/Birchmount_Park" title="Birchmount Park">Birchmount Park</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2K</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Bayview_Village" title="Bayview Village">Bayview Village</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3K</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Downsview" title="Downsview">Downsview</a>)<br/>East <br/> (<a class="mw-redirect" href="/wiki/Canadian_Forces_Base_Toronto" title="Canadian Forces Base Toronto">CFB Toronto</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4K</b><br/><span style="font-size:85%;">East Toronto<br/>(<a class="mw-redirect" href="/wiki/The_Danforth" title="The Danforth">The Danforth</a> West / <a href="/wiki/Riverdale,_Toronto" title="Riverdale, Toronto">Riverdale</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5K</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a class="mw-redirect" href="/wiki/Toronto_Dominion_Centre" title="Toronto Dominion Centre">Toronto Dominion Centre</a> / <a href="/wiki/Design_Exchange" title="Design Exchange">Design Exchange</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6K</b><br/><span style="font-size:85%;">West Toronto<br/>(<a class="mw-redirect" href="/wiki/Brockton,_Toronto" title="Brockton, Toronto">Brockton</a> / <a class="mw-redirect" href="/wiki/Parkdale_Village" title="Parkdale Village">Parkdale Village</a> / <a href="/wiki/Exhibition_Place" title="Exhibition Place">Exhibition Place</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7K</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8K</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9K</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1L</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Golden_Mile,_Toronto" title="Golden Mile, Toronto">Golden Mile</a> / <a href="/wiki/Clairlea" title="Clairlea">Clairlea</a> / <a href="/wiki/Oakridge,_Toronto" title="Oakridge, Toronto">Oakridge</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2L</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/York_Mills" title="York Mills">York Mills</a> / Silver Hills)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3L</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Downsview" title="Downsview">Downsview</a>)<br/>West</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4L</b><br/><span style="font-size:85%;">East Toronto<br/>(<a href="/wiki/Gerrard_Street_(Toronto)" title="Gerrard Street (Toronto)">India Bazaar</a> / <a href="/wiki/The_Beaches" title="The Beaches">The Beaches</a> West)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5L</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/Commerce_Court" title="Commerce Court">Commerce Court</a> / <a href="/wiki/Hotel_Victoria_(Toronto)" title="Hotel Victoria (Toronto)">Victoria Hotel</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6L</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(North Park / <a href="/wiki/Maple_Leaf,_Toronto" title="Maple Leaf, Toronto">Maple Leaf Park</a> / Upwood Park)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7L</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8L</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9L</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Humber_Summit" title="Humber Summit">Humber Summit</a>)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1M</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Cliffside,_Toronto" title="Cliffside, Toronto">Cliffside</a> / <a href="/wiki/Cliffcrest" title="Cliffcrest">Cliffcrest</a> / <a href="/wiki/Scarborough_Village" title="Scarborough Village">Scarborough Village</a> West)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2M</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(Willowdale / <a href="/wiki/Newtonbrook" title="Newtonbrook">Newtonbrook</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3M</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Downsview" title="Downsview">Downsview</a>)<br/>Central</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4M</b><br/><span style="font-size:85%;">East Toronto<br/>(Studio District)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5M</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Bedford_Park,_Toronto" title="Bedford Park, Toronto">Bedford Park</a> / <a href="/wiki/Lawrence_Manor" title="Lawrence Manor">Lawrence Manor</a> East)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6M</b><br/><span style="font-size:85%;"><a href="/wiki/York,_Toronto" title="York, Toronto">York</a><br/>(Del Ray / <a href="/wiki/Mount_Dennis" title="Mount Dennis">Mount Dennis</a> / <a href="/wiki/Silverthorn,_Toronto" title="Silverthorn, Toronto">Keelsdale and Silverthorn</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7M</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8M</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9M</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a class="mw-redirect" href="/wiki/Humberlea" title="Humberlea">Humberlea</a> / <a class="mw-redirect" href="/wiki/Emery,_Toronto" title="Emery, Toronto">Emery</a>)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1N</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Birch_Cliff" title="Birch Cliff">Birch Cliff</a> / <a href="/wiki/Cliffside,_Toronto" title="Cliffside, Toronto">Cliffside</a> West)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2N</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Willowdale,_Toronto" title="Willowdale, Toronto">Willowdale</a>)<br/>South</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M3N</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Downsview" title="Downsview">Downsview</a>)<br/>Northwest</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4N</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/Lawrence_Park,_Toronto" title="Lawrence Park, Toronto">Lawrence Park</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5N</b><br/><span style="font-size:85%;">Central Toronto<br/>(Roselawn)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6N</b><br/><span style="font-size:85%;"><a href="/wiki/York,_Toronto" title="York, Toronto">York</a><br/>(<a href="/wiki/Runnymede,_Toronto" title="Runnymede, Toronto">Runnymede</a> / <a href="/wiki/The_Junction" title="The Junction">The Junction</a> North)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7N</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8N</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9N</b><br/><span style="font-size:85%;"><a href="/wiki/York,_Toronto" title="York, Toronto">York</a><br/>(<a href="/wiki/Weston,_Toronto" title="Weston, Toronto">Weston</a>)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1P</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Dorset_Park" title="Dorset Park">Dorset Park</a> / <a class="mw-redirect" href="/wiki/Wexford_Heights" title="Wexford Heights">Wexford Heights</a> / <a href="/wiki/Scarborough_Town_Centre" title="Scarborough Town Centre">Scarborough Town Centre</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2P</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/York_Mills" title="York Mills">York Mills</a> West)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3P</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4P</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/Davisville_Village" title="Davisville Village">Davisville</a> North)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5P</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/Forest_Hill,_Toronto" title="Forest Hill, Toronto">Forest Hill</a> North &amp; West)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6P</b><br/><span style="font-size:85%;">West Toronto<br/>(<a href="/wiki/High_Park" title="High Park">High Park</a> / <a href="/wiki/The_Junction" title="The Junction">The Junction</a> South)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7P</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8P</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9P</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(Westmount)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1R</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Wexford,_Toronto" title="Wexford, Toronto">Wexford</a> / <a href="/wiki/Maryvale,_Toronto" title="Maryvale, Toronto">Maryvale</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M2R</b><br/><span style="font-size:85%;"><a href="/wiki/North_York" title="North York">North York</a><br/>(<a href="/wiki/Willowdale,_Toronto" title="Willowdale, Toronto">Willowdale</a>)<br/>West</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3R</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4R</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/North_Toronto" title="North Toronto">North Toronto</a> West)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5R</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/The_Annex" title="The Annex">The Annex</a> / <a class="mw-redirect" href="/wiki/North_Midtown,_Toronto" title="North Midtown, Toronto">North Midtown</a> / <a href="/wiki/Yorkville,_Toronto" title="Yorkville, Toronto">Yorkville</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6R</b><br/><span style="font-size:85%;">West Toronto<br/>(<a href="/wiki/Parkdale,_Toronto" title="Parkdale, Toronto">Parkdale</a> / <a href="/wiki/Roncesvalles,_Toronto" title="Roncesvalles, Toronto">Roncesvalles</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M7R</b><br/><span style="font-size:85%;"><a href="/wiki/Mississauga" title="Mississauga">Mississauga</a><br/>Canada Post Gateway Processing Centre<br/>(Enclave of L4W)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8R</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9R</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a href="/wiki/Kingsview_Village" title="Kingsview Village">Kingsview Village</a> / St. Phillips / Martin Grove Gardens / Richview Gardens)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1S</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Agincourt,_Toronto" title="Agincourt, Toronto">Agincourt</a>) </span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2S</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3S</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4S</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/Davisville_Village" title="Davisville Village">Davisville</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5S</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/University_of_Toronto" title="University of Toronto">University of Toronto</a> / Harbord)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M6S</b><br/><span style="font-size:85%;">West Toronto<br/>(<a href="/wiki/Runnymede,_Toronto" title="Runnymede, Toronto">Runnymede</a> / <a href="/wiki/Swansea,_Toronto" title="Swansea, Toronto">Swansea</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7S</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8S</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9S</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1T</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(Clarks Corners / <a class="mw-redirect" href="/wiki/Tam_O%27Shanter,_Ontario" title="Tam O'Shanter, Ontario">Tam O'Shanter</a> / <a class="mw-redirect" href="/wiki/Sullivan,_Toronto" title="Sullivan, Toronto">Sullivan</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2T</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3T</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4T</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/Moore_Park,_Toronto" title="Moore Park, Toronto">Moore Park</a> / <a href="/wiki/Summerhill,_Toronto" title="Summerhill, Toronto">Summerhill</a> East)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5T</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/Kensington_Market" title="Kensington Market">Kensington Market</a> / <a href="/wiki/Chinatown,_Toronto" title="Chinatown, Toronto">Chinatown</a> / <a href="/wiki/Grange_Park_(Toronto)" title="Grange Park (Toronto)">Grange Park</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M6T</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7T</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M8T</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9T</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1V</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a class="mw-redirect" href="/wiki/Milliken,_Toronto" title="Milliken, Toronto">Milliken</a> / <a class="mw-redirect" href="/wiki/Agincourt_North" title="Agincourt North">Agincourt North</a> / <a href="/wiki/Steeles,_Toronto" title="Steeles, Toronto">Steeles</a> East / <a href="/wiki/L%27Amoreaux" title="L'Amoreaux">L'Amoreaux</a> East)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2V</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3V</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4V</b><br/><span style="font-size:85%;">Central Toronto<br/>(<a href="/wiki/Summerhill,_Toronto" title="Summerhill, Toronto">Summerhill</a> West / <a class="mw-redirect" href="/wiki/Rathnelly" title="Rathnelly">Rathnelly</a> / <a href="/wiki/South_Hill,_Toronto" title="South Hill, Toronto">South Hill</a> / <a href="/wiki/Forest_Hill,_Toronto" title="Forest Hill, Toronto">Forest Hill</a> SE / <a href="/wiki/Deer_Park,_Toronto" title="Deer Park, Toronto">Deer Park</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5V</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/CN_Tower" title="CN Tower">CN Tower</a> / <a class="mw-redirect" href="/wiki/King_and_Spadina" title="King and Spadina">King and Spadina</a> / <a href="/wiki/Railway_Lands" title="Railway Lands">Railway Lands</a> / <a class="mw-redirect" href="/wiki/Harbourfront_(Toronto)" title="Harbourfront (Toronto)">Harbourfront</a> West / Bathurst Quay / <a class="mw-redirect" href="/wiki/South_Niagara" title="South Niagara">South Niagara</a> / <a href="/wiki/Billy_Bishop_Toronto_City_Airport" title="Billy Bishop Toronto City Airport">Island airport</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M6V</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7V</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M8V</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a href="/wiki/New_Toronto" title="New Toronto">New Toronto</a> / <a href="/wiki/Mimico" title="Mimico">Mimico</a> South / Humber Bay Shores)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9V</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a class="mw-redirect" href="/wiki/South_Steeles" title="South Steeles">South Steeles</a> / <a class="mw-redirect" href="/wiki/Silverstone,_Toronto" title="Silverstone, Toronto">Silverstone</a> / Humbergate / <a class="mw-redirect" href="/wiki/Jamestown,_Toronto" title="Jamestown, Toronto">Jamestown</a> / <a class="mw-redirect" href="/wiki/Mount_Olive,_Toronto" title="Mount Olive, Toronto">Mount Olive</a> / <a class="mw-redirect" href="/wiki/Beaumond_Heights" title="Beaumond Heights">Beaumond Heights</a> / <a href="/wiki/Thistletown" title="Thistletown">Thistletown</a> / Albion Gardens)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1W</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a href="/wiki/Steeles,_Toronto" title="Steeles, Toronto">Steeles</a> West / <a href="/wiki/L%27Amoreaux" title="L'Amoreaux">L'Amoreaux</a> West)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2W</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3W</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4W</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/Rosedale,_Toronto" title="Rosedale, Toronto">Rosedale</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5W</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>Stn A PO Boxes<br/>25 The Esplanade<br/>(Enclave of M5E)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M6W</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7W</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M8W</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a href="/wiki/Alderwood,_Toronto" title="Alderwood, Toronto">Alderwood</a> / <a href="/wiki/Long_Branch,_Toronto" title="Long Branch, Toronto">Long Branch</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M9W</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>Northwest<br/>(<a class="mw-redirect" href="/wiki/Clairville,_Toronto" title="Clairville, Toronto">Clairville</a> / <a href="/wiki/Humberwood" title="Humberwood">Humberwood</a> / <a class="mw-redirect" href="/wiki/Woodbine_Downs" title="Woodbine Downs">Woodbine Downs</a> / West Humber / Kipling Heights / <a href="/wiki/Rexdale" title="Rexdale">Rexdale</a> / <a class="mw-redirect" href="/wiki/Elms,_Toronto" title="Elms, Toronto">Elms</a> / Tandridge / Old Rexdale)</span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top;">
<p><b>M1X</b><br/><span style="font-size:85%;"><a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">Scarborough</a><br/>(<a class="mw-redirect" href="/wiki/Upper_Rouge" title="Upper Rouge">Upper Rouge</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2X</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3X</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4X</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/St._James_Town" title="St. James Town">St. James Town</a> / <a class="mw-redirect" href="/wiki/Cabbagetown_(Toronto)" title="Cabbagetown (Toronto)">Cabbagetown</a>)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M5X</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/First_Canadian_Place" title="First Canadian Place">First Canadian Place</a> / <a href="/wiki/PATH_(Toronto)" title="PATH (Toronto)">Underground city</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M6X</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7X</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M8X</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a href="/wiki/The_Kingsway,_Toronto" title="The Kingsway, Toronto">The Kingsway</a> / Montgomery Road / <a href="/wiki/Old_Mill,_Toronto" title="Old Mill, Toronto">Old Mill</a> North)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9X</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top; color:#ccc;">
<p><b>M1Y</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2Y</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3Y</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M4Y</b><br/><span style="font-size:85%;"><a href="/wiki/Downtown_Toronto" title="Downtown Toronto">Downtown Toronto</a><br/>(<a href="/wiki/Church_and_Wellesley" title="Church and Wellesley">Church and Wellesley</a>)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M5Y</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M6Y</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M7Y</b><br/><span style="font-size:85%;">East Toronto<br/><a class="mw-redirect" href="/wiki/Business_reply_mail" title="Business reply mail">Business reply mail</a> Processing Centre<br/>969 Eastern<br/>(Enclave of M4L)</span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M8Y</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a href="/wiki/Old_Mill,_Toronto" title="Old Mill, Toronto">Old Mill</a> South / King's Mill Park / <a class="mw-redirect" href="/wiki/Sunnylea" title="Sunnylea">Sunnylea</a> / <a href="/wiki/Humber_Bay" title="Humber Bay">Humber Bay</a> / <a href="/wiki/Mimico" title="Mimico">Mimico</a> NE / <a class="mw-redirect" href="/wiki/The_Queensway_(Toronto)" title="The Queensway (Toronto)">The Queensway</a> East / Royal York South East / Kingsway Park South East)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9Y</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr>
<tr>
<td style="vertical-align:top; color:#ccc;">
<p><b>M1Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M2Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M3Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M4Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M5Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M6Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M7Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td>
<td style="vertical-align:top;">
<p><b>M8Z</b><br/><span style="font-size:85%;"><a href="/wiki/Etobicoke" title="Etobicoke">Etobicoke</a><br/>(<a href="/wiki/Mimico" title="Mimico">Mimico</a> NW / <a href="/wiki/The_Queensway" title="The Queensway">The Queensway</a> West / South of Bloor / Kingsway Park South West / Royal York South West)</span>
</p>
</td>
<td style="vertical-align:top; color:#ccc;">
<p><b>M9Z</b><br/><span style="font-size:85%;"><i>Not assigned</i></span>
</p>
</td></tr></tbody></table>



```python
# puts data in the correct format

table_contents=[]
table=soup.find('table')
for row in table.findAll('td'):
    cell = {}
    if row.span.text=='Not assigned':
        pass
    else:
        cell['PostalCode'] = row.p.text[:3]
        cell['Borough'] = (row.span.text).split('(')[0]
        cell['Neighborhood'] = (((((row.span.text).split('(')[1]).strip(')')).replace(' /',',')).replace(')',' ')).strip(' ')
        table_contents.append({'Postal Code':cell['PostalCode'],'Borough':cell['Borough'],'Neighborhood':cell['Neighborhood']})
```


```python
df = pd.DataFrame(table_contents)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Ontario Provincial Government</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = pd.DataFrame(table_contents,columns = ['Postal Code','Borough','Neighborhood'])
df2['Borough']=df2['Borough'].replace({'Downtown TorontoStn A PO Boxes25 The Esplanade':'Downtown Toronto Stn A',
                                             'East TorontoBusiness reply mail Processing Centre969 Eastern':'East Toronto Business',
                                             'EtobicokeNorthwest':'Etobicoke Northwest','East YorkEast Toronto':'East York/East Toronto',
                                             'MississaugaCanada Post Gateway Processing Centre':'Mississauga'})
```


```python
df2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Ontario Provincial Government</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.shape
```




    (103, 3)



# Getting the latitude and longitude of each neighborhood


```python
# create a dataframe with the postal code data from the csv file

locgeo_df = pd.read_csv('https://cocl.us/Geospatial_data', index_col='Postal Code')
locgeo_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
    <tr>
      <th>Postal Code</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>M1B</th>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>M1C</th>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>M1E</th>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>M1G</th>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>M1H</th>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>




```python
# grouping the original dataframe by postcode

df_postcode = df2.groupby(['Postal Code', 'Borough'])['Neighborhood'].apply(', '.join).reset_index()
df_postcode.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Malvern, Rouge</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Rouge Hill, Port Union, Highland Creek</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>Scarborough</td>
      <td>Guildwood, Morningside, West Hill</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>Scarborough</td>
      <td>Woburn</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>Scarborough</td>
      <td>Cedarbrae</td>
    </tr>
  </tbody>
</table>
</div>




```python
# merging the dataframes 

final_data = df_postcode.join(locgeo_df, on='Postal Code') 
final_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Malvern, Rouge</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Rouge Hill, Port Union, Highland Creek</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>Scarborough</td>
      <td>Guildwood, Morningside, West Hill</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>Scarborough</td>
      <td>Woburn</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>Scarborough</td>
      <td>Cedarbrae</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>



# Create Map of Toronto Neighborhoods


```python
# Make a dataframe with just the boroughs that contain the word Toronto

toronto_data = final_data[final_data['Borough'].str.contains('Toronto',regex=False)]
toronto_data

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37</th>
      <td>M4E</td>
      <td>East Toronto</td>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
    </tr>
    <tr>
      <th>40</th>
      <td>M4J</td>
      <td>East York/East Toronto</td>
      <td>The Danforth  East</td>
      <td>43.685347</td>
      <td>-79.338106</td>
    </tr>
    <tr>
      <th>41</th>
      <td>M4K</td>
      <td>East Toronto</td>
      <td>The Danforth West, Riverdale</td>
      <td>43.679557</td>
      <td>-79.352188</td>
    </tr>
    <tr>
      <th>42</th>
      <td>M4L</td>
      <td>East Toronto</td>
      <td>India Bazaar, The Beaches West</td>
      <td>43.668999</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>43</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td>Studio District</td>
      <td>43.659526</td>
      <td>-79.340923</td>
    </tr>
    <tr>
      <th>44</th>
      <td>M4N</td>
      <td>Central Toronto</td>
      <td>Lawrence Park</td>
      <td>43.728020</td>
      <td>-79.388790</td>
    </tr>
    <tr>
      <th>45</th>
      <td>M4P</td>
      <td>Central Toronto</td>
      <td>Davisville North</td>
      <td>43.712751</td>
      <td>-79.390197</td>
    </tr>
    <tr>
      <th>46</th>
      <td>M4R</td>
      <td>Central Toronto</td>
      <td>North Toronto West</td>
      <td>43.715383</td>
      <td>-79.405678</td>
    </tr>
    <tr>
      <th>47</th>
      <td>M4S</td>
      <td>Central Toronto</td>
      <td>Davisville</td>
      <td>43.704324</td>
      <td>-79.388790</td>
    </tr>
    <tr>
      <th>48</th>
      <td>M4T</td>
      <td>Central Toronto</td>
      <td>Moore Park, Summerhill East</td>
      <td>43.689574</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>49</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td>Summerhill West, Rathnelly, South Hill, Forest...</td>
      <td>43.686412</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>50</th>
      <td>M4W</td>
      <td>Downtown Toronto</td>
      <td>Rosedale</td>
      <td>43.679563</td>
      <td>-79.377529</td>
    </tr>
    <tr>
      <th>51</th>
      <td>M4X</td>
      <td>Downtown Toronto</td>
      <td>St. James Town, Cabbagetown</td>
      <td>43.667967</td>
      <td>-79.367675</td>
    </tr>
    <tr>
      <th>52</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
      <td>43.665860</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>53</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>54</th>
      <td>M5B</td>
      <td>Downtown Toronto</td>
      <td>Garden District, Ryerson</td>
      <td>43.657162</td>
      <td>-79.378937</td>
    </tr>
    <tr>
      <th>55</th>
      <td>M5C</td>
      <td>Downtown Toronto</td>
      <td>St. James Town</td>
      <td>43.651494</td>
      <td>-79.375418</td>
    </tr>
    <tr>
      <th>56</th>
      <td>M5E</td>
      <td>Downtown Toronto</td>
      <td>Berczy Park</td>
      <td>43.644771</td>
      <td>-79.373306</td>
    </tr>
    <tr>
      <th>57</th>
      <td>M5G</td>
      <td>Downtown Toronto</td>
      <td>Central Bay Street</td>
      <td>43.657952</td>
      <td>-79.387383</td>
    </tr>
    <tr>
      <th>58</th>
      <td>M5H</td>
      <td>Downtown Toronto</td>
      <td>Richmond, Adelaide, King</td>
      <td>43.650571</td>
      <td>-79.384568</td>
    </tr>
    <tr>
      <th>59</th>
      <td>M5J</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront East, Union Station, Toronto Islands</td>
      <td>43.640816</td>
      <td>-79.381752</td>
    </tr>
    <tr>
      <th>60</th>
      <td>M5K</td>
      <td>Downtown Toronto</td>
      <td>Toronto Dominion Centre, Design Exchange</td>
      <td>43.647177</td>
      <td>-79.381576</td>
    </tr>
    <tr>
      <th>61</th>
      <td>M5L</td>
      <td>Downtown Toronto</td>
      <td>Commerce Court, Victoria Hotel</td>
      <td>43.648198</td>
      <td>-79.379817</td>
    </tr>
    <tr>
      <th>63</th>
      <td>M5N</td>
      <td>Central Toronto</td>
      <td>Roselawn</td>
      <td>43.711695</td>
      <td>-79.416936</td>
    </tr>
    <tr>
      <th>64</th>
      <td>M5P</td>
      <td>Central Toronto</td>
      <td>Forest Hill North &amp; West</td>
      <td>43.696948</td>
      <td>-79.411307</td>
    </tr>
    <tr>
      <th>65</th>
      <td>M5R</td>
      <td>Central Toronto</td>
      <td>The Annex, North Midtown, Yorkville</td>
      <td>43.672710</td>
      <td>-79.405678</td>
    </tr>
    <tr>
      <th>66</th>
      <td>M5S</td>
      <td>Downtown Toronto</td>
      <td>University of Toronto, Harbord</td>
      <td>43.662696</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>67</th>
      <td>M5T</td>
      <td>Downtown Toronto</td>
      <td>Kensington Market, Chinatown, Grange Park</td>
      <td>43.653206</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>68</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
    </tr>
    <tr>
      <th>69</th>
      <td>M5W</td>
      <td>Downtown Toronto Stn A</td>
      <td>Enclave of M5E</td>
      <td>43.646435</td>
      <td>-79.374846</td>
    </tr>
    <tr>
      <th>70</th>
      <td>M5X</td>
      <td>Downtown Toronto</td>
      <td>First Canadian Place, Underground city</td>
      <td>43.648429</td>
      <td>-79.382280</td>
    </tr>
    <tr>
      <th>75</th>
      <td>M6G</td>
      <td>Downtown Toronto</td>
      <td>Christie</td>
      <td>43.669542</td>
      <td>-79.422564</td>
    </tr>
    <tr>
      <th>76</th>
      <td>M6H</td>
      <td>West Toronto</td>
      <td>Dufferin, Dovercourt Village</td>
      <td>43.669005</td>
      <td>-79.442259</td>
    </tr>
    <tr>
      <th>77</th>
      <td>M6J</td>
      <td>West Toronto</td>
      <td>Little Portugal, Trinity</td>
      <td>43.647927</td>
      <td>-79.419750</td>
    </tr>
    <tr>
      <th>78</th>
      <td>M6K</td>
      <td>West Toronto</td>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>43.636847</td>
      <td>-79.428191</td>
    </tr>
    <tr>
      <th>82</th>
      <td>M6P</td>
      <td>West Toronto</td>
      <td>High Park, The Junction South</td>
      <td>43.661608</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>83</th>
      <td>M6R</td>
      <td>West Toronto</td>
      <td>Parkdale, Roncesvalles</td>
      <td>43.648960</td>
      <td>-79.456325</td>
    </tr>
    <tr>
      <th>84</th>
      <td>M6S</td>
      <td>West Toronto</td>
      <td>Runnymede, Swansea</td>
      <td>43.651571</td>
      <td>-79.484450</td>
    </tr>
    <tr>
      <th>87</th>
      <td>M7Y</td>
      <td>East Toronto Business</td>
      <td>Enclave of M4L</td>
      <td>43.662744</td>
      <td>-79.321558</td>
    </tr>
  </tbody>
</table>
</div>




```python
# get the coordinates of Toronto

address = 'Toronto, CA'

geolocator = Nominatim(user_agent="to_explorer")
location = geolocator.geocode(address)
latitude = location.latitude
longitude = location.longitude
print('The geograpical coordinate of Toronto are {}, {}.'.format(latitude, longitude))
```

    The geograpical coordinate of Toronto are 43.6534817, -79.3839347.



```python
# create the map

map_toronto = folium.Map(location=[latitude, longitude],zoom_start=11)

for lat,lng,borough,neighbourhood in zip(toronto_data['Latitude'], toronto_data['Longitude'], toronto_data['Borough'], toronto_data['Neighborhood']):
    label = '{}, {}'.format(neighbourhood, borough)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
    [lat,lng],
    radius=5,
    popup=label,
    color='blue',
    fill=True,
    fill_color='#3186cc',
    fill_opacity=0.7,
    parse_html=False).add_to(map_toronto)
map_toronto
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=%3C%21DOCTYPE%20html%3E%0A%3Chead%3E%20%20%20%20%0A%20%20%20%20%3Cmeta%20http-equiv%3D%22content-type%22%20content%3D%22text/html%3B%20charset%3DUTF-8%22%20/%3E%0A%20%20%20%20%3Cscript%3EL_PREFER_CANVAS%20%3D%20false%3B%20L_NO_TOUCH%20%3D%20false%3B%20L_DISABLE_3D%20%3D%20false%3B%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js%22%3E%3C/script%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//rawgit.com/python-visualization/folium/master/folium/templates/leaflet.awesome.rotate.css%22/%3E%0A%20%20%20%20%3Cstyle%3Ehtml%2C%20body%20%7Bwidth%3A%20100%25%3Bheight%3A%20100%25%3Bmargin%3A%200%3Bpadding%3A%200%3B%7D%3C/style%3E%0A%20%20%20%20%3Cstyle%3E%23map%20%7Bposition%3Aabsolute%3Btop%3A0%3Bbottom%3A0%3Bright%3A0%3Bleft%3A0%3B%7D%3C/style%3E%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cstyle%3E%20%23map_ee7358b6d8cd498a95a86dd88672ccb4%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20position%20%3A%20relative%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20width%20%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20height%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20left%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20top%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%3C/style%3E%0A%20%20%20%20%20%20%20%20%0A%3C/head%3E%0A%3Cbody%3E%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cdiv%20class%3D%22folium-map%22%20id%3D%22map_ee7358b6d8cd498a95a86dd88672ccb4%22%20%3E%3C/div%3E%0A%20%20%20%20%20%20%20%20%0A%3C/body%3E%0A%3Cscript%3E%20%20%20%20%0A%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20bounds%20%3D%20null%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20map_ee7358b6d8cd498a95a86dd88672ccb4%20%3D%20L.map%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27map_ee7358b6d8cd498a95a86dd88672ccb4%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7Bcenter%3A%20%5B43.6534817%2C-79.3839347%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20zoom%3A%2011%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20maxBounds%3A%20bounds%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20layers%3A%20%5B%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20worldCopyJump%3A%20false%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20crs%3A%20L.CRS.EPSG3857%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20tile_layer_7d354d27f6a344f9afe6428143761bc7%20%3D%20L.tileLayer%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27https%3A//%7Bs%7D.tile.openstreetmap.org/%7Bz%7D/%7Bx%7D/%7By%7D.png%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22attribution%22%3A%20null%2C%0A%20%20%22detectRetina%22%3A%20false%2C%0A%20%20%22maxZoom%22%3A%2018%2C%0A%20%20%22minZoom%22%3A%201%2C%0A%20%20%22noWrap%22%3A%20false%2C%0A%20%20%22subdomains%22%3A%20%22abc%22%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_f8d8716aef4f4eb6bf84d964aea89eca%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6763574%2C-79.2930312%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_09dcbd1940bd492ba547f392eeae58be%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f7dad5bb518a4bcbb493762f1d096ffd%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f7dad5bb518a4bcbb493762f1d096ffd%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Beaches%2C%20East%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_09dcbd1940bd492ba547f392eeae58be.setContent%28html_f7dad5bb518a4bcbb493762f1d096ffd%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_f8d8716aef4f4eb6bf84d964aea89eca.bindPopup%28popup_09dcbd1940bd492ba547f392eeae58be%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9ac88130a29641d8bbd5d96fed422b05%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.685347%2C-79.3381065%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_e1f8c2d6175e42fd8fb9fd9af476bb94%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_d15d79eb541f442d9ead345ce7650833%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_d15d79eb541f442d9ead345ce7650833%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Danforth%20%20East%2C%20East%20York/East%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_e1f8c2d6175e42fd8fb9fd9af476bb94.setContent%28html_d15d79eb541f442d9ead345ce7650833%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9ac88130a29641d8bbd5d96fed422b05.bindPopup%28popup_e1f8c2d6175e42fd8fb9fd9af476bb94%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_6bbe7090faa4450782dd28973986c4b4%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6795571%2C-79.352188%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f794c6ab571b4a259057c587e02e9599%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_61153d11e0e9437493c8c9f1618c22a1%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_61153d11e0e9437493c8c9f1618c22a1%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Danforth%20West%2C%20Riverdale%2C%20East%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f794c6ab571b4a259057c587e02e9599.setContent%28html_61153d11e0e9437493c8c9f1618c22a1%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_6bbe7090faa4450782dd28973986c4b4.bindPopup%28popup_f794c6ab571b4a259057c587e02e9599%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_0405a254124a45e3a7b16147ebcb6ccb%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6689985%2C-79.3155716%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_0248add2fb93457eae3a6cd86147f43e%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e96ad7ab4c3a4996a89d9f9e052ac661%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e96ad7ab4c3a4996a89d9f9e052ac661%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EIndia%20Bazaar%2C%20The%20Beaches%20West%2C%20East%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_0248add2fb93457eae3a6cd86147f43e.setContent%28html_e96ad7ab4c3a4996a89d9f9e052ac661%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_0405a254124a45e3a7b16147ebcb6ccb.bindPopup%28popup_0248add2fb93457eae3a6cd86147f43e%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_ce2518f60e4e4d579b7e94f75ba5128d%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6595255%2C-79.340923%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d1907687b83c41a394e6b5716ca4d9f2%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_fde69c01de9d4462ad1fc79e73302168%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_fde69c01de9d4462ad1fc79e73302168%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EStudio%20District%2C%20East%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d1907687b83c41a394e6b5716ca4d9f2.setContent%28html_fde69c01de9d4462ad1fc79e73302168%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_ce2518f60e4e4d579b7e94f75ba5128d.bindPopup%28popup_d1907687b83c41a394e6b5716ca4d9f2%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_108929a3ca5344349b239f6a29315380%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7280205%2C-79.3887901%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d59a96f67a6d40dca78944de97b7f7ed%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_a733b1b02d184e0d9b5fa7c847feeb7f%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_a733b1b02d184e0d9b5fa7c847feeb7f%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ELawrence%20Park%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d59a96f67a6d40dca78944de97b7f7ed.setContent%28html_a733b1b02d184e0d9b5fa7c847feeb7f%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_108929a3ca5344349b239f6a29315380.bindPopup%28popup_d59a96f67a6d40dca78944de97b7f7ed%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_42368028ff824a07b1ad0dac12a996e7%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7127511%2C-79.3901975%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d7508790618241c0aef016c2792e553a%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f29e0546e11e4e329117babf85eaac48%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f29e0546e11e4e329117babf85eaac48%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EDavisville%20North%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d7508790618241c0aef016c2792e553a.setContent%28html_f29e0546e11e4e329117babf85eaac48%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_42368028ff824a07b1ad0dac12a996e7.bindPopup%28popup_d7508790618241c0aef016c2792e553a%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_13ff195c42f84794ba9542f3154ae2c7%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7153834%2C-79.4056784%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_2a89efcb71fe40999e24885ca4edd116%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_a0914ff9448b43d2b66b986a148008e4%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_a0914ff9448b43d2b66b986a148008e4%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENorth%20Toronto%20West%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_2a89efcb71fe40999e24885ca4edd116.setContent%28html_a0914ff9448b43d2b66b986a148008e4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_13ff195c42f84794ba9542f3154ae2c7.bindPopup%28popup_2a89efcb71fe40999e24885ca4edd116%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_126737e8833348f3b9cf201785767254%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7043244%2C-79.3887901%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_23347b52757243c7bf8089a40b82478f%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_5a60802c5e3a49e4a12778de5c622776%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_5a60802c5e3a49e4a12778de5c622776%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EDavisville%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_23347b52757243c7bf8089a40b82478f.setContent%28html_5a60802c5e3a49e4a12778de5c622776%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_126737e8833348f3b9cf201785767254.bindPopup%28popup_23347b52757243c7bf8089a40b82478f%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_95443936c47d4c04b6bf2228d2bb52f3%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6895743%2C-79.3831599%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_b68ccd2b9af84baab939eed446a3cffb%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_94f808a2961b4d8b8e3a5d869a1e57f4%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_94f808a2961b4d8b8e3a5d869a1e57f4%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EMoore%20Park%2C%20Summerhill%20East%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_b68ccd2b9af84baab939eed446a3cffb.setContent%28html_94f808a2961b4d8b8e3a5d869a1e57f4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_95443936c47d4c04b6bf2228d2bb52f3.bindPopup%28popup_b68ccd2b9af84baab939eed446a3cffb%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_f284561373274b99903615b7aa977c50%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6864123%2C-79.4000493%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_df9a32ae7a3846e08a1170a8b18989ad%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_03f0f6185c38483ca825818c63c56d52%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_03f0f6185c38483ca825818c63c56d52%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ESummerhill%20West%2C%20Rathnelly%2C%20South%20Hill%2C%20Forest%20Hill%20SE%2C%20Deer%20Park%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_df9a32ae7a3846e08a1170a8b18989ad.setContent%28html_03f0f6185c38483ca825818c63c56d52%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_f284561373274b99903615b7aa977c50.bindPopup%28popup_df9a32ae7a3846e08a1170a8b18989ad%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_e1bba75ec5e64094b07b423d95108343%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6795626%2C-79.3775294%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_7792de96267c4581b2e3383663bf9d22%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_2176de2595e64a5d91814c3ad227d78e%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_2176de2595e64a5d91814c3ad227d78e%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERosedale%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_7792de96267c4581b2e3383663bf9d22.setContent%28html_2176de2595e64a5d91814c3ad227d78e%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_e1bba75ec5e64094b07b423d95108343.bindPopup%28popup_7792de96267c4581b2e3383663bf9d22%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_febddf70bf9a417ea4ee1019d9b0aadb%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.667967%2C-79.3676753%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f80505f39e9a4a27a351dd24e757c498%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_d1811a9ae62141669215974630cccb53%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_d1811a9ae62141669215974630cccb53%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ESt.%20James%20Town%2C%20Cabbagetown%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f80505f39e9a4a27a351dd24e757c498.setContent%28html_d1811a9ae62141669215974630cccb53%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_febddf70bf9a417ea4ee1019d9b0aadb.bindPopup%28popup_f80505f39e9a4a27a351dd24e757c498%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b9ef6a95894f43789389111d73cce512%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6658599%2C-79.3831599%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_ac11305f9a204b4fb24e674995b78f87%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_37e91eea4de24254a6d0747cbe7bbcd2%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_37e91eea4de24254a6d0747cbe7bbcd2%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EChurch%20and%20Wellesley%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_ac11305f9a204b4fb24e674995b78f87.setContent%28html_37e91eea4de24254a6d0747cbe7bbcd2%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b9ef6a95894f43789389111d73cce512.bindPopup%28popup_ac11305f9a204b4fb24e674995b78f87%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_279e3042e71c4d0ba84ea1224dfe96e8%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6542599%2C-79.3606359%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_05b1ea46572148d383b596ceb3b65cb7%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_727fe7ef68044cb1928a7fe433eda4e8%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_727fe7ef68044cb1928a7fe433eda4e8%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERegent%20Park%2C%20Harbourfront%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_05b1ea46572148d383b596ceb3b65cb7.setContent%28html_727fe7ef68044cb1928a7fe433eda4e8%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_279e3042e71c4d0ba84ea1224dfe96e8.bindPopup%28popup_05b1ea46572148d383b596ceb3b65cb7%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_ece394ea4b9640398bba9229a2a5a744%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6571618%2C-79.3789371%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_38d056bf02aa4d00adb284cff6d94f67%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_52ce04eed59b4fe4bfb1150ecb4298dc%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_52ce04eed59b4fe4bfb1150ecb4298dc%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EGarden%20District%2C%20Ryerson%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_38d056bf02aa4d00adb284cff6d94f67.setContent%28html_52ce04eed59b4fe4bfb1150ecb4298dc%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_ece394ea4b9640398bba9229a2a5a744.bindPopup%28popup_38d056bf02aa4d00adb284cff6d94f67%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b7c6acc2fa4741629182f0ab298f94bc%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6514939%2C-79.3754179%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_af647fea640f4a11b62af4d8c35c9fc1%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_164d3680b0b847a7a40e719c0abd1e23%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_164d3680b0b847a7a40e719c0abd1e23%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ESt.%20James%20Town%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_af647fea640f4a11b62af4d8c35c9fc1.setContent%28html_164d3680b0b847a7a40e719c0abd1e23%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b7c6acc2fa4741629182f0ab298f94bc.bindPopup%28popup_af647fea640f4a11b62af4d8c35c9fc1%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_2428d3248a02411da0317c5f535ec3c1%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6447708%2C-79.3733064%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_9c1a08b2c5ed4836bfcd642d72921823%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_3206058d77f74d3ca792dd88c0fdf011%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_3206058d77f74d3ca792dd88c0fdf011%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EBerczy%20Park%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_9c1a08b2c5ed4836bfcd642d72921823.setContent%28html_3206058d77f74d3ca792dd88c0fdf011%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_2428d3248a02411da0317c5f535ec3c1.bindPopup%28popup_9c1a08b2c5ed4836bfcd642d72921823%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_d3662e025f654fb49e34a49cadf503e3%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6579524%2C-79.3873826%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_26832d2c25d64d6586a0a886d741d157%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_943cddad88994eb381d93928e4af27c5%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_943cddad88994eb381d93928e4af27c5%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ECentral%20Bay%20Street%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_26832d2c25d64d6586a0a886d741d157.setContent%28html_943cddad88994eb381d93928e4af27c5%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_d3662e025f654fb49e34a49cadf503e3.bindPopup%28popup_26832d2c25d64d6586a0a886d741d157%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_357a7e6838954b40880dec8557ac01cf%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6505712%2C-79.3845675%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_73447639538d4b43b3f7b0299c46bbca%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_fd239bccaad44beb891cedb9c36395df%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_fd239bccaad44beb891cedb9c36395df%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERichmond%2C%20Adelaide%2C%20King%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_73447639538d4b43b3f7b0299c46bbca.setContent%28html_fd239bccaad44beb891cedb9c36395df%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_357a7e6838954b40880dec8557ac01cf.bindPopup%28popup_73447639538d4b43b3f7b0299c46bbca%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9b32f173d93b424fab562b52ec03baa6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6408157%2C-79.3817523%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c291f3a79a134fad8046fcdf37c92287%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_8f61aed58726405d955fa0e962528d91%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_8f61aed58726405d955fa0e962528d91%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EHarbourfront%20East%2C%20Union%20Station%2C%20Toronto%20Islands%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c291f3a79a134fad8046fcdf37c92287.setContent%28html_8f61aed58726405d955fa0e962528d91%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9b32f173d93b424fab562b52ec03baa6.bindPopup%28popup_c291f3a79a134fad8046fcdf37c92287%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_6357e6b39e644293a4f5d39f351d0396%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6471768%2C-79.3815764%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_3ff8659e3cf24c1f9ccb6487a99d2686%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_12a0ee2c771e4253b5101b6cfd82feb6%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_12a0ee2c771e4253b5101b6cfd82feb6%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EToronto%20Dominion%20Centre%2C%20Design%20Exchange%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_3ff8659e3cf24c1f9ccb6487a99d2686.setContent%28html_12a0ee2c771e4253b5101b6cfd82feb6%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_6357e6b39e644293a4f5d39f351d0396.bindPopup%28popup_3ff8659e3cf24c1f9ccb6487a99d2686%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_445bf29b10fa4a38bd5eb411345e443d%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6481985%2C-79.3798169%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_aaa786be53cd4ab4b9d1e9865023dfa8%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_be5a2eaa9827470e8e63ea0b22868fa7%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_be5a2eaa9827470e8e63ea0b22868fa7%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ECommerce%20Court%2C%20Victoria%20Hotel%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_aaa786be53cd4ab4b9d1e9865023dfa8.setContent%28html_be5a2eaa9827470e8e63ea0b22868fa7%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_445bf29b10fa4a38bd5eb411345e443d.bindPopup%28popup_aaa786be53cd4ab4b9d1e9865023dfa8%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_daae4e63c40e4940b28b4c7b617239be%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7116948%2C-79.4169356%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_85c427abc7054b71afc615b14ab2ec38%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_117b6ef557714b44beb425d9ae99b04a%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_117b6ef557714b44beb425d9ae99b04a%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERoselawn%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_85c427abc7054b71afc615b14ab2ec38.setContent%28html_117b6ef557714b44beb425d9ae99b04a%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_daae4e63c40e4940b28b4c7b617239be.bindPopup%28popup_85c427abc7054b71afc615b14ab2ec38%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_caa56b2f725c4325898164f0b55428dd%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6969476%2C-79.4113072%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_b82c5e3f1b264156a91aed6558f4440b%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_2c8b81871f1c4b3b9090f0b9651d4674%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_2c8b81871f1c4b3b9090f0b9651d4674%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EForest%20Hill%20North%20%26amp%3B%20West%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_b82c5e3f1b264156a91aed6558f4440b.setContent%28html_2c8b81871f1c4b3b9090f0b9651d4674%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_caa56b2f725c4325898164f0b55428dd.bindPopup%28popup_b82c5e3f1b264156a91aed6558f4440b%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_4855b1100a9b464eb069ee8fc3cc7145%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6727097%2C-79.4056784%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_fc4b599e61954a62a633c716985a6763%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_8e97989f1f9e4d42b18c4820d4ffc5ef%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_8e97989f1f9e4d42b18c4820d4ffc5ef%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Annex%2C%20North%20Midtown%2C%20Yorkville%2C%20Central%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_fc4b599e61954a62a633c716985a6763.setContent%28html_8e97989f1f9e4d42b18c4820d4ffc5ef%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_4855b1100a9b464eb069ee8fc3cc7145.bindPopup%28popup_fc4b599e61954a62a633c716985a6763%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_a0293216f61944fc92ae816020490616%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6626956%2C-79.4000493%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_edd25fadf9ff42afa92c48fffb103b2f%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_23a24e9525704c0ca388960202b8f737%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_23a24e9525704c0ca388960202b8f737%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EUniversity%20of%20Toronto%2C%20Harbord%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_edd25fadf9ff42afa92c48fffb103b2f.setContent%28html_23a24e9525704c0ca388960202b8f737%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_a0293216f61944fc92ae816020490616.bindPopup%28popup_edd25fadf9ff42afa92c48fffb103b2f%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_ec27e7c0ca124ee4bd63fbef231cbbdd%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6532057%2C-79.4000493%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_ec60fdf74eac43d1961e92a4f71ee84e%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_b616181e6c2145ee828972e7328abd90%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_b616181e6c2145ee828972e7328abd90%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EKensington%20Market%2C%20Chinatown%2C%20Grange%20Park%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_ec60fdf74eac43d1961e92a4f71ee84e.setContent%28html_b616181e6c2145ee828972e7328abd90%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_ec27e7c0ca124ee4bd63fbef231cbbdd.bindPopup%28popup_ec60fdf74eac43d1961e92a4f71ee84e%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_eb718bc11a4e446e8a1766568bde6689%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6289467%2C-79.3944199%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_dd125b78f46a4f30b40201cca6316891%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_47a947d2687e4d7cbd7904a32d300108%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_47a947d2687e4d7cbd7904a32d300108%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ECN%20Tower%2C%20King%20and%20Spadina%2C%20Railway%20Lands%2C%20Harbourfront%20West%2C%20Bathurst%20Quay%2C%20South%20Niagara%2C%20Island%20airport%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_dd125b78f46a4f30b40201cca6316891.setContent%28html_47a947d2687e4d7cbd7904a32d300108%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_eb718bc11a4e446e8a1766568bde6689.bindPopup%28popup_dd125b78f46a4f30b40201cca6316891%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_614cbc392eaa4ec48bcff6db2067b5e6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6464352%2C-79.374846%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c9f3afbb3cad42289e06e723143f15ff%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_7143517d69244877a004f4057ffc26db%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_7143517d69244877a004f4057ffc26db%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EEnclave%20of%20M5E%2C%20Downtown%20Toronto%20Stn%20A%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c9f3afbb3cad42289e06e723143f15ff.setContent%28html_7143517d69244877a004f4057ffc26db%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_614cbc392eaa4ec48bcff6db2067b5e6.bindPopup%28popup_c9f3afbb3cad42289e06e723143f15ff%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_3885eba462b644638522ec6c0dd8e74f%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6484292%2C-79.3822802%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_20819ab9e333494a8001bf8084959e38%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_83bf0d68b98840b18735b7897058bf57%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_83bf0d68b98840b18735b7897058bf57%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EFirst%20Canadian%20Place%2C%20Underground%20city%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_20819ab9e333494a8001bf8084959e38.setContent%28html_83bf0d68b98840b18735b7897058bf57%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_3885eba462b644638522ec6c0dd8e74f.bindPopup%28popup_20819ab9e333494a8001bf8084959e38%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c080f3c8dcde4429b31f29bb0fd86829%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.669542%2C-79.4225637%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_a8ead69465734eb39984a1d6df504d48%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_24fd413a0d9143c295e6d39054a40910%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_24fd413a0d9143c295e6d39054a40910%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EChristie%2C%20Downtown%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_a8ead69465734eb39984a1d6df504d48.setContent%28html_24fd413a0d9143c295e6d39054a40910%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c080f3c8dcde4429b31f29bb0fd86829.bindPopup%28popup_a8ead69465734eb39984a1d6df504d48%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_7333fdba77bc43ccaa2721782a0316a7%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6690051%2C-79.4422593%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_9f02c2c8d41643c4bd4b95bbe7382fd5%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_25e5db3b90c14d5ca656ec38072ac520%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_25e5db3b90c14d5ca656ec38072ac520%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EDufferin%2C%20Dovercourt%20Village%2C%20West%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_9f02c2c8d41643c4bd4b95bbe7382fd5.setContent%28html_25e5db3b90c14d5ca656ec38072ac520%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_7333fdba77bc43ccaa2721782a0316a7.bindPopup%28popup_9f02c2c8d41643c4bd4b95bbe7382fd5%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_0c607ac0e8214f62af0ac0695988cde6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6479267%2C-79.4197497%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f87dd5a6879d4cc084c2c744673fe137%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_8daddd6d79404eb38b310fd688308169%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_8daddd6d79404eb38b310fd688308169%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ELittle%20Portugal%2C%20Trinity%2C%20West%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f87dd5a6879d4cc084c2c744673fe137.setContent%28html_8daddd6d79404eb38b310fd688308169%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_0c607ac0e8214f62af0ac0695988cde6.bindPopup%28popup_f87dd5a6879d4cc084c2c744673fe137%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9fbeeb4051ab49848310a6c19a296d1a%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6368472%2C-79.4281914%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_1b11278a69244b6e9a1a0f60d4a093a4%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_319d758e85de4ce0a563214d06b567d9%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_319d758e85de4ce0a563214d06b567d9%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EBrockton%2C%20Parkdale%20Village%2C%20Exhibition%20Place%2C%20West%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_1b11278a69244b6e9a1a0f60d4a093a4.setContent%28html_319d758e85de4ce0a563214d06b567d9%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9fbeeb4051ab49848310a6c19a296d1a.bindPopup%28popup_1b11278a69244b6e9a1a0f60d4a093a4%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_3a32b637075c484b961aa0aaf261e3ec%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6616083%2C-79.4647633%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_194a936878fd45c4a265067dec61eb76%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_872a48c0e1a746db895c8cc485384599%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_872a48c0e1a746db895c8cc485384599%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EHigh%20Park%2C%20The%20Junction%20South%2C%20West%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_194a936878fd45c4a265067dec61eb76.setContent%28html_872a48c0e1a746db895c8cc485384599%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_3a32b637075c484b961aa0aaf261e3ec.bindPopup%28popup_194a936878fd45c4a265067dec61eb76%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_012f6a1b8603458d9cebab0caf6e0855%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6489597%2C-79.456325%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_b177e0b4d5374c20ab6001e3aa5bfe91%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_5bbc80b497924a32b482fe0400c42b6a%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_5bbc80b497924a32b482fe0400c42b6a%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EParkdale%2C%20Roncesvalles%2C%20West%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_b177e0b4d5374c20ab6001e3aa5bfe91.setContent%28html_5bbc80b497924a32b482fe0400c42b6a%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_012f6a1b8603458d9cebab0caf6e0855.bindPopup%28popup_b177e0b4d5374c20ab6001e3aa5bfe91%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_562cec684eb740b6a1d1dbe9051fcbb7%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6515706%2C-79.4844499%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_0b8a3cbf51554eeaa697c227c59e10d6%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_64e2eec331d242fcb0d8247342711501%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_64e2eec331d242fcb0d8247342711501%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERunnymede%2C%20Swansea%2C%20West%20Toronto%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_0b8a3cbf51554eeaa697c227c59e10d6.setContent%28html_64e2eec331d242fcb0d8247342711501%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_562cec684eb740b6a1d1dbe9051fcbb7.bindPopup%28popup_0b8a3cbf51554eeaa697c227c59e10d6%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_19203a939f644bab85cbe358c4ebf6b1%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6627439%2C-79.321558%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%233186cc%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_ee7358b6d8cd498a95a86dd88672ccb4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_ff2216b8b8f640fdad7d1e4284f103c9%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_1b9252fd92ce4570b618b599310a57e1%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_1b9252fd92ce4570b618b599310a57e1%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EEnclave%20of%20M4L%2C%20East%20Toronto%20Business%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_ff2216b8b8f640fdad7d1e4284f103c9.setContent%28html_1b9252fd92ce4570b618b599310a57e1%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_19203a939f644bab85cbe358c4ebf6b1.bindPopup%28popup_ff2216b8b8f640fdad7d1e4284f103c9%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%3C/script%3E onload="this.contentDocument.open();this.contentDocument.write(    decodeURIComponent(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



# Exploring the first Toronto neighborhoods


```python
CLIENT_ID = '0QBDDIG2ZXVQAYANLVFQOZYJ2YABNSHTBPNLEYHSJLCNWXF0' # your Foursquare ID
CLIENT_SECRET = 'XGDAWV2DJRFAKJPZ00SXIELF5W1GJO4RR3I5JQAZM1IWZMTD' # your Foursquare Secret
VERSION = '20180605' # Foursquare API version
LIMIT = 100 # A default Foursquare API limit value

print('Your credentails:')
print('CLIENT_ID: ' + CLIENT_ID)
print('CLIENT_SECRET:' + CLIENT_SECRET)
```

    Your credentails:
    CLIENT_ID: 0QBDDIG2ZXVQAYANLVFQOZYJ2YABNSHTBPNLEYHSJLCNWXF0
    CLIENT_SECRET:XGDAWV2DJRFAKJPZ00SXIELF5W1GJO4RR3I5JQAZM1IWZMTD



```python
# get the coordinates of the first neighborhood in the dataframe

neighborhood1 = toronto_data.loc[37, 'Neighborhood']

neighborhood_latitude = toronto_data.loc[37, 'Latitude'] # neighborhood latitude value
neighborhood_longitude = toronto_data.loc[37, 'Longitude'] # neighborhood longitude value

neighborhood_name = neighborhood1 # neighborhood name

print('Latitude and longitude values of {} are {}, {}.'.format(neighborhood_name, 
                                                               neighborhood_latitude, 
                                                               neighborhood_longitude))
```

    Latitude and longitude values of The Beaches are 43.6763574, -79.2930312.



```python
# create the GET request URL for the neighborhood

LIMIT = 100 # limit of number of venues returned by Foursquare API



radius = 500 # define radius


url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
    CLIENT_ID, 
    CLIENT_SECRET, 
    VERSION, 
    neighborhood_latitude, 
    neighborhood_longitude, 
    radius, 
    LIMIT)
url # display URL
```




    'https://api.foursquare.com/v2/venues/explore?&client_id=0QBDDIG2ZXVQAYANLVFQOZYJ2YABNSHTBPNLEYHSJLCNWXF0&client_secret=XGDAWV2DJRFAKJPZ00SXIELF5W1GJO4RR3I5JQAZM1IWZMTD&v=20180605&ll=43.6763574,-79.2930312&radius=500&limit=100'




```python
# send the GET request and examine the results

results = requests.get(url).json()
results
```




    {'meta': {'code': 200, 'requestId': '60af052578d1ac301d5260af'},
     'response': {'headerLocation': 'The Beaches',
      'headerFullLocation': 'The Beaches, Toronto',
      'headerLocationGranularity': 'neighborhood',
      'totalResults': 4,
      'suggestedBounds': {'ne': {'lat': 43.6808574045, 'lng': -79.28682091449052},
       'sw': {'lat': 43.6718573955, 'lng': -79.29924148550948}},
      'groups': [{'type': 'Recommended Places',
        'name': 'recommended',
        'items': [{'reasons': {'count': 0,
           'items': [{'summary': 'This spot is popular',
             'type': 'general',
             'reasonName': 'globalInteractionReason'}]},
          'venue': {'id': '4bd461bc77b29c74a07d9282',
           'name': 'Glen Manor Ravine',
           'location': {'address': 'Glen Manor',
            'crossStreet': 'Queen St.',
            'lat': 43.67682094413784,
            'lng': -79.29394208780985,
            'labeledLatLngs': [{'label': 'display',
              'lat': 43.67682094413784,
              'lng': -79.29394208780985}],
            'distance': 89,
            'cc': 'CA',
            'city': 'Toronto',
            'state': 'ON',
            'country': 'Canada',
            'formattedAddress': ['Glen Manor (Queen St.)',
             'Toronto ON',
             'Canada']},
           'categories': [{'id': '4bf58dd8d48988d159941735',
             'name': 'Trail',
             'pluralName': 'Trails',
             'shortName': 'Trail',
             'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/parks_outdoors/hikingtrail_',
              'suffix': '.png'},
             'primary': True}],
           'photos': {'count': 0, 'groups': []}},
          'referralId': 'e-0-4bd461bc77b29c74a07d9282-0'},
         {'reasons': {'count': 0,
           'items': [{'summary': 'This spot is popular',
             'type': 'general',
             'reasonName': 'globalInteractionReason'}]},
          'venue': {'id': '4ad4c062f964a52011f820e3',
           'name': 'The Big Carrot Natural Food Market',
           'location': {'address': '125 Southwood Dr',
            'lat': 43.678879,
            'lng': -79.297734,
            'labeledLatLngs': [{'label': 'display',
              'lat': 43.678879,
              'lng': -79.297734}],
            'distance': 471,
            'postalCode': 'M4E 0B8',
            'cc': 'CA',
            'city': 'Toronto',
            'state': 'ON',
            'country': 'Canada',
            'formattedAddress': ['125 Southwood Dr',
             'Toronto ON M4E 0B8',
             'Canada']},
           'categories': [{'id': '50aa9e744b90af0d42d5de0e',
             'name': 'Health Food Store',
             'pluralName': 'Health Food Stores',
             'shortName': 'Health Food Store',
             'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/shops/food_grocery_',
              'suffix': '.png'},
             'primary': True}],
           'photos': {'count': 0, 'groups': []},
           'venuePage': {'id': '75150878'}},
          'referralId': 'e-0-4ad4c062f964a52011f820e3-1'},
         {'reasons': {'count': 0,
           'items': [{'summary': 'This spot is popular',
             'type': 'general',
             'reasonName': 'globalInteractionReason'}]},
          'venue': {'id': '4b8daea1f964a520480833e3',
           'name': 'Grover Pub and Grub',
           'location': {'address': '676 Kingston Rd.',
            'crossStreet': 'at Main St.',
            'lat': 43.679181434941015,
            'lng': -79.29721535878515,
            'labeledLatLngs': [{'label': 'display',
              'lat': 43.679181434941015,
              'lng': -79.29721535878515}],
            'distance': 460,
            'postalCode': 'M4E 1R4',
            'cc': 'CA',
            'city': 'Toronto',
            'state': 'ON',
            'country': 'Canada',
            'formattedAddress': ['676 Kingston Rd. (at Main St.)',
             'Toronto ON M4E 1R4',
             'Canada']},
           'categories': [{'id': '4bf58dd8d48988d11b941735',
             'name': 'Pub',
             'pluralName': 'Pubs',
             'shortName': 'Pub',
             'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/nightlife/pub_',
              'suffix': '.png'},
             'primary': True}],
           'photos': {'count': 0, 'groups': []}},
          'referralId': 'e-0-4b8daea1f964a520480833e3-2'},
         {'reasons': {'count': 0,
           'items': [{'summary': 'This spot is popular',
             'type': 'general',
             'reasonName': 'globalInteractionReason'}]},
          'venue': {'id': '4df91c4bae60f95f82229ad5',
           'name': 'Upper Beaches',
           'location': {'lat': 43.68056321147582,
            'lng': -79.2928688743688,
            'labeledLatLngs': [{'label': 'display',
              'lat': 43.68056321147582,
              'lng': -79.2928688743688}],
            'distance': 468,
            'cc': 'CA',
            'city': 'Toronto',
            'state': 'ON',
            'country': 'Canada',
            'formattedAddress': ['Toronto ON', 'Canada']},
           'categories': [{'id': '4f2a25ac4b909258e854f55f',
             'name': 'Neighborhood',
             'pluralName': 'Neighborhoods',
             'shortName': 'Neighborhood',
             'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/parks_outdoors/neighborhood_',
              'suffix': '.png'},
             'primary': True}],
           'photos': {'count': 0, 'groups': []}},
          'referralId': 'e-0-4df91c4bae60f95f82229ad5-3'}]}]}}




```python
# create a function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']
        
    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']
```


```python
# clean the json and structure it into a pandas dataframe

venues = results['response']['groups'][0]['items']
    
nearby_venues = json_normalize(venues) # flatten JSON

# filter columns
filtered_columns = ['venue.name', 'venue.categories', 'venue.location.lat', 'venue.location.lng']
nearby_venues =nearby_venues.loc[:, filtered_columns]

# filter the category for each row
nearby_venues['venue.categories'] = nearby_venues.apply(get_category_type, axis=1)

# clean columns
nearby_venues.columns = [col.split(".")[-1] for col in nearby_venues.columns]

nearby_venues
```

    /opt/conda/envs/Python-3.7-main/lib/python3.7/site-packages/ipykernel/__main__.py:5: FutureWarning: pandas.io.json.json_normalize is deprecated, use pandas.json_normalize instead





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>categories</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Glen Manor Ravine</td>
      <td>Trail</td>
      <td>43.676821</td>
      <td>-79.293942</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Big Carrot Natural Food Market</td>
      <td>Health Food Store</td>
      <td>43.678879</td>
      <td>-79.297734</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Grover Pub and Grub</td>
      <td>Pub</td>
      <td>43.679181</td>
      <td>-79.297215</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Upper Beaches</td>
      <td>Neighborhood</td>
      <td>43.680563</td>
      <td>-79.292869</td>
    </tr>
  </tbody>
</table>
</div>




```python
# see how many values were returned by Foursquare

print('{} venues were returned by Foursquare.'.format(nearby_venues.shape[0]))
```

    4 venues were returned by Foursquare.


# Exploring all the Toronto neighborhoods


```python
# create a function that repeats the process for all Toronto neighborhoods

def getNearbyVenues(names, latitudes, longitudes, radius=500):
    
    venues_list=[]
    for name, lat, lng in zip(names, latitudes, longitudes):
        print(name)
            
        # create the API request URL
        url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
            CLIENT_ID, 
            CLIENT_SECRET, 
            VERSION, 
            lat, 
            lng, 
            radius, 
            LIMIT)
            
        # make the GET request
        results = requests.get(url).json()["response"]['groups'][0]['items']
        
               # return only relevant information for each nearby venue
        venues_list.append([(
            name, 
            lat, 
            lng, 
            v['venue']['name'], 
            v['venue']['location']['lat'], 
            v['venue']['location']['lng'],  
            v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['Neighborhood', 
                  'Neighborhood Latitude', 
                  'Neighborhood Longitude', 
                  'Venue', 
                  'Venue Latitude', 
                  'Venue Longitude', 
                  'Venue Category']
    
    return(nearby_venues)
```


```python
# run the above function on each neighborhood and create new dataframe
toronto_venues = getNearbyVenues(names=toronto_data['Neighborhood'],
                                   latitudes=toronto_data['Latitude'],
                                   longitudes=toronto_data['Longitude']
                                  )
```

    The Beaches
    The Danforth  East
    The Danforth West, Riverdale
    India Bazaar, The Beaches West
    Studio District
    Lawrence Park
    Davisville North
    North Toronto West
    Davisville
    Moore Park, Summerhill East
    Summerhill West, Rathnelly, South Hill, Forest Hill SE, Deer Park
    Rosedale
    St. James Town, Cabbagetown
    Church and Wellesley
    Regent Park, Harbourfront
    Garden District, Ryerson
    St. James Town
    Berczy Park
    Central Bay Street
    Richmond, Adelaide, King
    Harbourfront East, Union Station, Toronto Islands
    Toronto Dominion Centre, Design Exchange
    Commerce Court, Victoria Hotel
    Roselawn
    Forest Hill North & West
    The Annex, North Midtown, Yorkville
    University of Toronto, Harbord
    Kensington Market, Chinatown, Grange Park
    CN Tower, King and Spadina, Railway Lands, Harbourfront West, Bathurst Quay, South Niagara, Island airport
    Enclave of M5E
    First Canadian Place, Underground city
    Christie
    Dufferin, Dovercourt Village
    Little Portugal, Trinity
    Brockton, Parkdale Village, Exhibition Place
    High Park, The Junction South
    Parkdale, Roncesvalles
    Runnymede, Swansea
    Enclave of M4L



```python
# check how many venues were returned for each neighborhood

toronto_venues.groupby('Neighborhood').count()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Neighborhood Latitude</th>
      <th>Neighborhood Longitude</th>
      <th>Venue</th>
      <th>Venue Latitude</th>
      <th>Venue Longitude</th>
      <th>Venue Category</th>
    </tr>
    <tr>
      <th>Neighborhood</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Berczy Park</th>
      <td>59</td>
      <td>59</td>
      <td>59</td>
      <td>59</td>
      <td>59</td>
      <td>59</td>
    </tr>
    <tr>
      <th>Brockton, Parkdale Village, Exhibition Place</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>CN Tower, King and Spadina, Railway Lands, Harbourfront West, Bathurst Quay, South Niagara, Island airport</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Central Bay Street</th>
      <td>62</td>
      <td>62</td>
      <td>62</td>
      <td>62</td>
      <td>62</td>
      <td>62</td>
    </tr>
    <tr>
      <th>Christie</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Church and Wellesley</th>
      <td>80</td>
      <td>80</td>
      <td>80</td>
      <td>80</td>
      <td>80</td>
      <td>80</td>
    </tr>
    <tr>
      <th>Commerce Court, Victoria Hotel</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Davisville</th>
      <td>35</td>
      <td>35</td>
      <td>35</td>
      <td>35</td>
      <td>35</td>
      <td>35</td>
    </tr>
    <tr>
      <th>Davisville North</th>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Dufferin, Dovercourt Village</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Enclave of M4L</th>
      <td>17</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
    </tr>
    <tr>
      <th>Enclave of M5E</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>First Canadian Place, Underground city</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Forest Hill North &amp; West</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Garden District, Ryerson</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Harbourfront East, Union Station, Toronto Islands</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>High Park, The Junction South</th>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
      <td>25</td>
    </tr>
    <tr>
      <th>India Bazaar, The Beaches West</th>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>Kensington Market, Chinatown, Grange Park</th>
      <td>63</td>
      <td>63</td>
      <td>63</td>
      <td>63</td>
      <td>63</td>
      <td>63</td>
    </tr>
    <tr>
      <th>Lawrence Park</th>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Little Portugal, Trinity</th>
      <td>44</td>
      <td>44</td>
      <td>44</td>
      <td>44</td>
      <td>44</td>
      <td>44</td>
    </tr>
    <tr>
      <th>Moore Park, Summerhill East</th>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>North Toronto West</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Parkdale, Roncesvalles</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>Regent Park, Harbourfront</th>
      <td>45</td>
      <td>45</td>
      <td>45</td>
      <td>45</td>
      <td>45</td>
      <td>45</td>
    </tr>
    <tr>
      <th>Richmond, Adelaide, King</th>
      <td>92</td>
      <td>92</td>
      <td>92</td>
      <td>92</td>
      <td>92</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Rosedale</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Roselawn</th>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Runnymede, Swansea</th>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
    </tr>
    <tr>
      <th>St. James Town</th>
      <td>82</td>
      <td>82</td>
      <td>82</td>
      <td>82</td>
      <td>82</td>
      <td>82</td>
    </tr>
    <tr>
      <th>St. James Town, Cabbagetown</th>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
      <td>46</td>
    </tr>
    <tr>
      <th>Studio District</th>
      <td>36</td>
      <td>36</td>
      <td>36</td>
      <td>36</td>
      <td>36</td>
      <td>36</td>
    </tr>
    <tr>
      <th>Summerhill West, Rathnelly, South Hill, Forest Hill SE, Deer Park</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>The Annex, North Midtown, Yorkville</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>The Beaches</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>The Danforth  East</th>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>The Danforth West, Riverdale</th>
      <td>41</td>
      <td>41</td>
      <td>41</td>
      <td>41</td>
      <td>41</td>
      <td>41</td>
    </tr>
    <tr>
      <th>Toronto Dominion Centre, Design Exchange</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>University of Toronto, Harbord</th>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>




```python
# find out how many unique categories can be curated from all the returned venues

print('There are {} unique categories.'.format(len(toronto_venues['Venue Category'].unique())))

```

    There are 236 unique categories.


# Analyzing each neighborhood


```python
# one hot encoding
toronto_onehot = pd.get_dummies(toronto_venues[['Venue Category']], prefix="", prefix_sep="")

# add neighborhood column back to dataframe
toronto_onehot['Neighborhood'] = toronto_venues['Neighborhood'] 

# move neighborhood column to the first column
fixed_columns = [toronto_onehot.columns[-1]] + list(toronto_onehot.columns[:-1])
toronto_onehot = toronto_onehot[fixed_columns]

toronto_onehot.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Yoga Studio</th>
      <th>Adult Boutique</th>
      <th>Afghan Restaurant</th>
      <th>Airport</th>
      <th>Airport Food Court</th>
      <th>Airport Lounge</th>
      <th>Airport Service</th>
      <th>Airport Terminal</th>
      <th>American Restaurant</th>
      <th>Antique Shop</th>
      <th>...</th>
      <th>Theme Restaurant</th>
      <th>Tibetan Restaurant</th>
      <th>Toy / Game Store</th>
      <th>Trail</th>
      <th>Train Station</th>
      <th>Vegetarian / Vegan Restaurant</th>
      <th>Video Game Store</th>
      <th>Vietnamese Restaurant</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 236 columns</p>
</div>




```python
# group rows by neighborhood and by taking the mean of the frequency of occurrence of each category

toronto_grouped = toronto_onehot.groupby('Neighborhood').mean().reset_index()
toronto_grouped
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Neighborhood</th>
      <th>Yoga Studio</th>
      <th>Adult Boutique</th>
      <th>Afghan Restaurant</th>
      <th>Airport</th>
      <th>Airport Food Court</th>
      <th>Airport Lounge</th>
      <th>Airport Service</th>
      <th>Airport Terminal</th>
      <th>American Restaurant</th>
      <th>...</th>
      <th>Theme Restaurant</th>
      <th>Tibetan Restaurant</th>
      <th>Toy / Game Store</th>
      <th>Trail</th>
      <th>Train Station</th>
      <th>Vegetarian / Vegan Restaurant</th>
      <th>Video Game Store</th>
      <th>Vietnamese Restaurant</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Berczy Park</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.016949</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0625</td>
      <td>0.0625</td>
      <td>0.125</td>
      <td>0.1875</td>
      <td>0.125</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Central Bay Street</td>
      <td>0.016129</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.016129</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.016129</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Christie</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Church and Wellesley</td>
      <td>0.025000</td>
      <td>0.0125</td>
      <td>0.0125</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.012500</td>
      <td>...</td>
      <td>0.0125</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Commerce Court, Victoria Hotel</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.030000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.020000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.010000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Davisville</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.028571</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.028571</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Davisville North</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Dufferin, Dovercourt Village</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Enclave of M4L</td>
      <td>0.058824</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Enclave of M5E</td>
      <td>0.010000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.010000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>First Canadian Place, Underground city</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.030000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.01</td>
      <td>0.010000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.010000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Forest Hill North &amp; West</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.250000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Garden District, Ryerson</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.010000</td>
      <td>0.010000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Harbourfront East, Union Station, Toronto Islands</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.01</td>
      <td>0.010000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.010000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>High Park, The Junction South</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>India Bazaar, The Beaches West</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Kensington Market, Chinatown, Grange Park</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.047619</td>
      <td>0.00000</td>
      <td>0.047619</td>
      <td>0.015873</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Lawrence Park</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Little Portugal, Trinity</td>
      <td>0.022727</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.045455</td>
      <td>0.00000</td>
      <td>0.045455</td>
      <td>0.022727</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Moore Park, Summerhill East</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>North Toronto West</td>
      <td>0.047619</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.047619</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Parkdale, Roncesvalles</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Regent Park, Harbourfront</td>
      <td>0.022222</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.022222</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Richmond, Adelaide, King</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.021739</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.010870</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Rosedale</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.250000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Roselawn</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Runnymede, Swansea</td>
      <td>0.031250</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.031250</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>St. James Town</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.024390</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.012195</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.012195</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>30</th>
      <td>St. James Town, Cabbagetown</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.021739</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Studio District</td>
      <td>0.027778</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.055556</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Summerhill West, Rathnelly, South Hill, Forest...</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.071429</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.071429</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>33</th>
      <td>The Annex, North Midtown, Yorkville</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>34</th>
      <td>The Beaches</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.250000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>35</th>
      <td>The Danforth  East</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>36</th>
      <td>The Danforth West, Riverdale</td>
      <td>0.024390</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.024390</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.02439</td>
      <td>0.000000</td>
      <td>0.024390</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Toronto Dominion Centre, Design Exchange</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.020000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.01</td>
      <td>0.010000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.010000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>University of Toronto, Harbord</td>
      <td>0.031250</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.03125</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>39 rows × 236 columns</p>
</div>




```python
# print each neighborhood along with the top 5 most common venues

num_top_venues = 5

for hood in toronto_grouped['Neighborhood']:
    print("----"+hood+"----")
    temp = toronto_grouped[toronto_grouped['Neighborhood'] == hood].T.reset_index()
    temp.columns = ['venue','freq']
    temp = temp.iloc[1:]
    temp['freq'] = temp['freq'].astype(float)
    temp = temp.round({'freq': 2})
    print(temp.sort_values('freq', ascending=False).reset_index(drop=True).head(num_top_venues))
    print('\n')
```

    ----Berczy Park----
                venue  freq
    0     Coffee Shop  0.10
    1    Cocktail Bar  0.07
    2          Bakery  0.05
    3  Farmers Market  0.03
    4        Beer Bar  0.03
    
    
    ----Brockton, Parkdale Village, Exhibition Place----
                venue  freq
    0            Café  0.14
    1  Breakfast Spot  0.09
    2     Coffee Shop  0.09
    3    Intersection  0.05
    4   Burrito Place  0.05
    
    
    ----CN Tower, King and Spadina, Railway Lands, Harbourfront West, Bathurst Quay, South Niagara, Island airport----
                  venue  freq
    0   Airport Service  0.19
    1    Airport Lounge  0.12
    2  Airport Terminal  0.12
    3             Plane  0.06
    4          Boutique  0.06
    
    
    ----Central Bay Street----
                    venue  freq
    0         Coffee Shop  0.18
    1                Café  0.06
    2      Sandwich Place  0.05
    3  Italian Restaurant  0.05
    4         Pizza Place  0.03
    
    
    ----Christie----
               venue  freq
    0  Grocery Store  0.25
    1           Café  0.19
    2           Park  0.12
    3      Nightclub  0.06
    4     Baby Store  0.06
    
    
    ----Church and Wellesley----
                     venue  freq
    0          Coffee Shop  0.08
    1  Japanese Restaurant  0.06
    2     Sushi Restaurant  0.06
    3           Restaurant  0.04
    4              Gay Bar  0.04
    
    
    ----Commerce Court, Victoria Hotel----
             venue  freq
    0  Coffee Shop  0.14
    1   Restaurant  0.07
    2        Hotel  0.06
    3         Café  0.06
    4          Gym  0.04
    
    
    ----Davisville----
                  venue  freq
    0      Dessert Shop  0.09
    1    Sandwich Place  0.09
    2               Gym  0.06
    3  Sushi Restaurant  0.06
    4              Café  0.06
    
    
    ----Davisville North----
                  venue  freq
    0               Gym  0.11
    1             Hotel  0.11
    2        Playground  0.11
    3              Park  0.11
    4  Department Store  0.11
    
    
    ----Dufferin, Dovercourt Village----
                       venue  freq
    0               Pharmacy  0.12
    1                 Bakery  0.12
    2              Pet Store  0.06
    3  Portuguese Restaurant  0.06
    4                   Park  0.06
    
    
    ----Enclave of M4L----
                  venue  freq
    0       Yoga Studio  0.06
    1     Auto Workshop  0.06
    2       Pizza Place  0.06
    3        Comic Shop  0.06
    4  Recording Studio  0.06
    
    
    ----Enclave of M5E----
                    venue  freq
    0         Coffee Shop  0.13
    1        Cocktail Bar  0.04
    2  Seafood Restaurant  0.04
    3  Italian Restaurant  0.03
    4                Café  0.03
    
    
    ----First Canadian Place, Underground city----
                     venue  freq
    0          Coffee Shop  0.11
    1                 Café  0.07
    2                Hotel  0.06
    3  Japanese Restaurant  0.04
    4           Restaurant  0.04
    
    
    ----Forest Hill North & West----
                  venue  freq
    0             Trail  0.25
    1     Jewelry Store  0.25
    2          Bus Line  0.25
    3  Sushi Restaurant  0.25
    4       Yoga Studio  0.00
    
    
    ----Garden District, Ryerson----
                           venue  freq
    0             Clothing Store  0.09
    1                Coffee Shop  0.09
    2             Cosmetics Shop  0.03
    3  Middle Eastern Restaurant  0.03
    4            Bubble Tea Shop  0.03
    
    
    ----Harbourfront East, Union Station, Toronto Islands----
                venue  freq
    0     Coffee Shop  0.13
    1        Aquarium  0.05
    2           Hotel  0.04
    3            Café  0.04
    4  Scenic Lookout  0.03
    
    
    ----High Park, The Junction South----
                    venue  freq
    0     Thai Restaurant  0.08
    1  Mexican Restaurant  0.08
    2                Café  0.08
    3       Grocery Store  0.08
    4           Bookstore  0.04
    
    
    ----India Bazaar, The Beaches West----
                      venue  freq
    0                  Park  0.10
    1  Fast Food Restaurant  0.10
    2                   Gym  0.05
    3    Italian Restaurant  0.05
    4           Pizza Place  0.05
    
    
    ----Kensington Market, Chinatown, Grange Park----
                               venue  freq
    0                           Café  0.08
    1                    Coffee Shop  0.06
    2          Vietnamese Restaurant  0.05
    3  Vegetarian / Vegan Restaurant  0.05
    4                 Farmers Market  0.03
    
    
    ----Lawrence Park----
                     venue  freq
    0                 Park  0.33
    1             Bus Line  0.33
    2          Swim School  0.33
    3          Yoga Studio  0.00
    4  Moroccan Restaurant  0.00
    
    
    ----Little Portugal, Trinity----
                  venue  freq
    0               Bar  0.09
    1       Coffee Shop  0.07
    2       Men's Store  0.05
    3              Café  0.05
    4  Asian Restaurant  0.05
    
    
    ----Moore Park, Summerhill East----
                     venue  freq
    0                 Park  0.33
    1                Trail  0.33
    2         Tennis Court  0.33
    3          Yoga Studio  0.00
    4  Moroccan Restaurant  0.00
    
    
    ----North Toronto West----
                    venue  freq
    0      Clothing Store  0.19
    1         Coffee Shop  0.10
    2         Yoga Studio  0.05
    3  Italian Restaurant  0.05
    4               Diner  0.05
    
    
    ----Parkdale, Roncesvalles----
                venue  freq
    0       Gift Shop  0.14
    1  Breakfast Spot  0.14
    2    Dessert Shop  0.07
    3             Bar  0.07
    4         Dog Run  0.07
    
    
    ----Regent Park, Harbourfront----
                venue  freq
    0     Coffee Shop  0.16
    1            Park  0.07
    2          Bakery  0.07
    3  Breakfast Spot  0.04
    4         Theater  0.04
    
    
    ----Richmond, Adelaide, King----
                venue  freq
    0     Coffee Shop  0.11
    1            Café  0.05
    2      Restaurant  0.04
    3  Clothing Store  0.03
    4             Gym  0.03
    
    
    ----Rosedale----
                     venue  freq
    0                 Park  0.50
    1           Playground  0.25
    2                Trail  0.25
    3          Yoga Studio  0.00
    4  Moroccan Restaurant  0.00
    
    
    ----Roselawn----
                     venue  freq
    0          Music Venue  0.33
    1         Home Service  0.33
    2               Garden  0.33
    3               Museum  0.00
    4  Martial Arts School  0.00
    
    
    ----Runnymede, Swansea----
                    venue  freq
    0                Café  0.09
    1  Italian Restaurant  0.06
    2    Sushi Restaurant  0.06
    3         Coffee Shop  0.06
    4                 Pub  0.06
    
    
    ----St. James Town----
              venue  freq
    0          Café  0.07
    1   Coffee Shop  0.07
    2     Gastropub  0.04
    3  Cocktail Bar  0.04
    4           Gym  0.02
    
    
    ----St. James Town, Cabbagetown----
                    venue  freq
    0                Café  0.07
    1         Coffee Shop  0.07
    2                 Pub  0.04
    3  Chinese Restaurant  0.04
    4            Pharmacy  0.04
    
    
    ----Studio District----
                     venue  freq
    0          Coffee Shop  0.08
    1                 Café  0.06
    2               Bakery  0.06
    3              Brewery  0.06
    4  American Restaurant  0.06
    
    
    ----Summerhill West, Rathnelly, South Hill, Forest Hill SE, Deer Park----
                     venue  freq
    0          Coffee Shop  0.14
    1   Light Rail Station  0.07
    2         Liquor Store  0.07
    3  Fried Chicken Joint  0.07
    4          Supermarket  0.07
    
    
    ----The Annex, North Midtown, Yorkville----
                   venue  freq
    0     Sandwich Place  0.14
    1               Café  0.14
    2        Coffee Shop  0.10
    3           Pharmacy  0.05
    4  Indian Restaurant  0.05
    
    
    ----The Beaches----
                   venue  freq
    0  Health Food Store  0.25
    1              Trail  0.25
    2                Pub  0.25
    3        Yoga Studio  0.00
    4      Movie Theater  0.00
    
    
    ----The Danforth  East----
                     venue  freq
    0                 Park  0.67
    1    Convenience Store  0.33
    2          Yoga Studio  0.00
    3        Movie Theater  0.00
    4  Martial Arts School  0.00
    
    
    ----The Danforth West, Riverdale----
                        venue  freq
    0        Greek Restaurant  0.17
    1             Coffee Shop  0.10
    2      Italian Restaurant  0.07
    3  Furniture / Home Store  0.05
    4          Ice Cream Shop  0.05
    
    
    ----Toronto Dominion Centre, Design Exchange----
                    venue  freq
    0         Coffee Shop  0.12
    1               Hotel  0.08
    2                Café  0.06
    3  Seafood Restaurant  0.03
    4          Restaurant  0.03
    
    
    ----University of Toronto, Harbord----
                     venue  freq
    0                 Café  0.16
    1               Bakery  0.06
    2                  Bar  0.06
    3  Japanese Restaurant  0.06
    4            Bookstore  0.06
    
    



```python
# make a dataframe with the top 10 venues for each neighborhood

# write a function to sort the venues in descending order

def return_most_common_venues(row, num_top_venues):
    row_categories = row.iloc[1:]
    row_categories_sorted = row_categories.sort_values(ascending=False)
    
    return row_categories_sorted.index.values[0:num_top_venues]

# create the new dataframe and display the top 10 venues for each neighborhood.

num_top_venues = 10

indicators = ['st', 'nd', 'rd']

# create columns according to number of top venues
columns = ['Neighborhood']
for ind in np.arange(num_top_venues):
    try:
        columns.append('{}{} Most Common Venue'.format(ind+1, indicators[ind]))
    except:
        columns.append('{}th Most Common Venue'.format(ind+1))

# create a new dataframe
neighborhoods_venues_sorted = pd.DataFrame(columns=columns)
neighborhoods_venues_sorted['Neighborhood'] = toronto_grouped['Neighborhood']

for ind in np.arange(toronto_grouped.shape[0]):
    neighborhoods_venues_sorted.iloc[ind, 1:] = return_most_common_venues(toronto_grouped.iloc[ind, :], num_top_venues)

neighborhoods_venues_sorted.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Neighborhood</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Berczy Park</td>
      <td>Coffee Shop</td>
      <td>Cocktail Bar</td>
      <td>Bakery</td>
      <td>Farmers Market</td>
      <td>Beer Bar</td>
      <td>Restaurant</td>
      <td>Pharmacy</td>
      <td>Seafood Restaurant</td>
      <td>Cheese Shop</td>
      <td>Sporting Goods Shop</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>Café</td>
      <td>Breakfast Spot</td>
      <td>Coffee Shop</td>
      <td>Intersection</td>
      <td>Burrito Place</td>
      <td>Stadium</td>
      <td>Bar</td>
      <td>Furniture / Home Store</td>
      <td>Bakery</td>
      <td>Nightclub</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>Airport Service</td>
      <td>Airport Lounge</td>
      <td>Airport Terminal</td>
      <td>Plane</td>
      <td>Boutique</td>
      <td>Sculpture Garden</td>
      <td>Rental Car Location</td>
      <td>Boat or Ferry</td>
      <td>Bar</td>
      <td>Harbor / Marina</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Central Bay Street</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Sandwich Place</td>
      <td>Italian Restaurant</td>
      <td>Pizza Place</td>
      <td>Bubble Tea Shop</td>
      <td>Thai Restaurant</td>
      <td>Burger Joint</td>
      <td>Salad Place</td>
      <td>Japanese Restaurant</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Christie</td>
      <td>Grocery Store</td>
      <td>Café</td>
      <td>Park</td>
      <td>Nightclub</td>
      <td>Baby Store</td>
      <td>Italian Restaurant</td>
      <td>Athletics &amp; Sports</td>
      <td>Coffee Shop</td>
      <td>Restaurant</td>
      <td>Candy Store</td>
    </tr>
  </tbody>
</table>
</div>



# Cluster Neighborhoods


```python
# set number of clusters
kclusters = 5

toronto_grouped_clustering = toronto_grouped.drop('Neighborhood', 1)

# run k-means clustering
kmeans = KMeans(n_clusters=kclusters, random_state=0).fit(toronto_grouped_clustering)

# check cluster labels generated for each row in the dataframe
kmeans.labels_[0:10] 
```




    array([3, 3, 3, 3, 3, 3, 3, 3, 3, 3], dtype=int32)




```python
# add clustering labels
neighborhoods_venues_sorted.insert(0, 'Cluster Labels', kmeans.labels_)

toronto_merged = toronto_data

# merge toronto_grouped with toronto_data to add latitude/longitude for each neighborhood
toronto_merged = toronto_merged.join(neighborhoods_venues_sorted.set_index('Neighborhood'), on='Neighborhood')

toronto_merged.head() 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Cluster Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37</th>
      <td>M4E</td>
      <td>East Toronto</td>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>0</td>
      <td>Health Food Store</td>
      <td>Trail</td>
      <td>Pub</td>
      <td>Yoga Studio</td>
      <td>Movie Theater</td>
      <td>Martial Arts School</td>
      <td>Mediterranean Restaurant</td>
      <td>Men's Store</td>
      <td>Metro Station</td>
      <td>Mexican Restaurant</td>
    </tr>
    <tr>
      <th>40</th>
      <td>M4J</td>
      <td>East York/East Toronto</td>
      <td>The Danforth  East</td>
      <td>43.685347</td>
      <td>-79.338106</td>
      <td>1</td>
      <td>Park</td>
      <td>Convenience Store</td>
      <td>Yoga Studio</td>
      <td>Movie Theater</td>
      <td>Martial Arts School</td>
      <td>Mediterranean Restaurant</td>
      <td>Men's Store</td>
      <td>Metro Station</td>
      <td>Mexican Restaurant</td>
      <td>Middle Eastern Restaurant</td>
    </tr>
    <tr>
      <th>41</th>
      <td>M4K</td>
      <td>East Toronto</td>
      <td>The Danforth West, Riverdale</td>
      <td>43.679557</td>
      <td>-79.352188</td>
      <td>3</td>
      <td>Greek Restaurant</td>
      <td>Coffee Shop</td>
      <td>Italian Restaurant</td>
      <td>Furniture / Home Store</td>
      <td>Ice Cream Shop</td>
      <td>Yoga Studio</td>
      <td>Restaurant</td>
      <td>Juice Bar</td>
      <td>Fruit &amp; Vegetable Store</td>
      <td>Frozen Yogurt Shop</td>
    </tr>
    <tr>
      <th>42</th>
      <td>M4L</td>
      <td>East Toronto</td>
      <td>India Bazaar, The Beaches West</td>
      <td>43.668999</td>
      <td>-79.315572</td>
      <td>3</td>
      <td>Park</td>
      <td>Fast Food Restaurant</td>
      <td>Gym</td>
      <td>Italian Restaurant</td>
      <td>Pizza Place</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Movie Theater</td>
      <td>Restaurant</td>
      <td>Sandwich Place</td>
    </tr>
    <tr>
      <th>43</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td>Studio District</td>
      <td>43.659526</td>
      <td>-79.340923</td>
      <td>3</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Bakery</td>
      <td>Brewery</td>
      <td>American Restaurant</td>
      <td>Gastropub</td>
      <td>Seafood Restaurant</td>
      <td>Middle Eastern Restaurant</td>
      <td>Cheese Shop</td>
      <td>Bookstore</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create map
map_clusters = folium.Map(location=[latitude, longitude], zoom_start=11)

# set color scheme for the clusters
x = np.arange(kclusters)
ys = [i + x + (i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# add markers to the map
markers_colors = []
for lat, lon, poi, cluster in zip(toronto_merged['Latitude'], toronto_merged['Longitude'], toronto_merged['Neighborhood'], toronto_merged['Cluster Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_clusters)
       
map_clusters
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=%3C%21DOCTYPE%20html%3E%0A%3Chead%3E%20%20%20%20%0A%20%20%20%20%3Cmeta%20http-equiv%3D%22content-type%22%20content%3D%22text/html%3B%20charset%3DUTF-8%22%20/%3E%0A%20%20%20%20%3Cscript%3EL_PREFER_CANVAS%20%3D%20false%3B%20L_NO_TOUCH%20%3D%20false%3B%20L_DISABLE_3D%20%3D%20false%3B%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js%22%3E%3C/script%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//rawgit.com/python-visualization/folium/master/folium/templates/leaflet.awesome.rotate.css%22/%3E%0A%20%20%20%20%3Cstyle%3Ehtml%2C%20body%20%7Bwidth%3A%20100%25%3Bheight%3A%20100%25%3Bmargin%3A%200%3Bpadding%3A%200%3B%7D%3C/style%3E%0A%20%20%20%20%3Cstyle%3E%23map%20%7Bposition%3Aabsolute%3Btop%3A0%3Bbottom%3A0%3Bright%3A0%3Bleft%3A0%3B%7D%3C/style%3E%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cstyle%3E%20%23map_21544e3e59dd4fa784bc97ff8b45fc33%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20position%20%3A%20relative%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20width%20%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20height%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20left%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20top%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%3C/style%3E%0A%20%20%20%20%20%20%20%20%0A%3C/head%3E%0A%3Cbody%3E%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cdiv%20class%3D%22folium-map%22%20id%3D%22map_21544e3e59dd4fa784bc97ff8b45fc33%22%20%3E%3C/div%3E%0A%20%20%20%20%20%20%20%20%0A%3C/body%3E%0A%3Cscript%3E%20%20%20%20%0A%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20bounds%20%3D%20null%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20map_21544e3e59dd4fa784bc97ff8b45fc33%20%3D%20L.map%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27map_21544e3e59dd4fa784bc97ff8b45fc33%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7Bcenter%3A%20%5B43.6534817%2C-79.3839347%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20zoom%3A%2011%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20maxBounds%3A%20bounds%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20layers%3A%20%5B%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20worldCopyJump%3A%20false%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20crs%3A%20L.CRS.EPSG3857%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20tile_layer_a1ca5b309b96482fb5a872bcffe23e7b%20%3D%20L.tileLayer%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27https%3A//%7Bs%7D.tile.openstreetmap.org/%7Bz%7D/%7Bx%7D/%7By%7D.png%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22attribution%22%3A%20null%2C%0A%20%20%22detectRetina%22%3A%20false%2C%0A%20%20%22maxZoom%22%3A%2018%2C%0A%20%20%22minZoom%22%3A%201%2C%0A%20%20%22noWrap%22%3A%20false%2C%0A%20%20%22subdomains%22%3A%20%22abc%22%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_48facddeb1444b669fbfd40b09231831%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6763574%2C-79.2930312%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%23ff0000%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%23ff0000%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_85288e9574a346f4a5494c513de1eecf%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_727346dbe8ff4594821c689e621d53e2%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_727346dbe8ff4594821c689e621d53e2%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Beaches%20Cluster%200%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_85288e9574a346f4a5494c513de1eecf.setContent%28html_727346dbe8ff4594821c689e621d53e2%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_48facddeb1444b669fbfd40b09231831.bindPopup%28popup_85288e9574a346f4a5494c513de1eecf%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9536b058688a400e938249229582f024%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.685347%2C-79.3381065%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%238000ff%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%238000ff%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f272f883db494bb7bdc9c69c568c0889%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_d56a99bcd56b46f8abe578d167d12261%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_d56a99bcd56b46f8abe578d167d12261%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Danforth%20%20East%20Cluster%201%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f272f883db494bb7bdc9c69c568c0889.setContent%28html_d56a99bcd56b46f8abe578d167d12261%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9536b058688a400e938249229582f024.bindPopup%28popup_f272f883db494bb7bdc9c69c568c0889%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_53263c5c6d304163978bb137cc5c6fa4%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6795571%2C-79.352188%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_44f3760d49bb48abb6781ef7bbba1764%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f54fb2a70d8c4ddaab91ef4c19b4fe52%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f54fb2a70d8c4ddaab91ef4c19b4fe52%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Danforth%20West%2C%20Riverdale%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_44f3760d49bb48abb6781ef7bbba1764.setContent%28html_f54fb2a70d8c4ddaab91ef4c19b4fe52%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_53263c5c6d304163978bb137cc5c6fa4.bindPopup%28popup_44f3760d49bb48abb6781ef7bbba1764%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_22fcfa7390694d2194467d92d5211986%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6689985%2C-79.3155716%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_6f450a7597b2470baa5242fd6205e7ac%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_da64c48c3d8d4a8883dee1863e804c35%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_da64c48c3d8d4a8883dee1863e804c35%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EIndia%20Bazaar%2C%20The%20Beaches%20West%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_6f450a7597b2470baa5242fd6205e7ac.setContent%28html_da64c48c3d8d4a8883dee1863e804c35%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_22fcfa7390694d2194467d92d5211986.bindPopup%28popup_6f450a7597b2470baa5242fd6205e7ac%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_5269d8abb3a64662b4238f3d5247ddef%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6595255%2C-79.340923%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_a4f8bb9bfbcf4c449fe5bd49825a4298%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_ddc4bbacc84046aa823d312ded597ab3%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_ddc4bbacc84046aa823d312ded597ab3%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EStudio%20District%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_a4f8bb9bfbcf4c449fe5bd49825a4298.setContent%28html_ddc4bbacc84046aa823d312ded597ab3%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_5269d8abb3a64662b4238f3d5247ddef.bindPopup%28popup_a4f8bb9bfbcf4c449fe5bd49825a4298%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b9d122f9cdf24203958ce7ce48a125af%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7280205%2C-79.3887901%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%23ffb360%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%23ffb360%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_06c0dd75410b4410a173449356a3b342%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_6af44e6d8e0f41babc57e1176f9b1011%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_6af44e6d8e0f41babc57e1176f9b1011%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ELawrence%20Park%20Cluster%204%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_06c0dd75410b4410a173449356a3b342.setContent%28html_6af44e6d8e0f41babc57e1176f9b1011%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b9d122f9cdf24203958ce7ce48a125af.bindPopup%28popup_06c0dd75410b4410a173449356a3b342%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_2b2622850a8d46e58a4c24a0590edb0b%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7127511%2C-79.3901975%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_979fd3462dbf48bebc6ca0dde5e0b7e0%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e5e0ed2ab14d4ed1ad5b6d6694b7f34b%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e5e0ed2ab14d4ed1ad5b6d6694b7f34b%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EDavisville%20North%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_979fd3462dbf48bebc6ca0dde5e0b7e0.setContent%28html_e5e0ed2ab14d4ed1ad5b6d6694b7f34b%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_2b2622850a8d46e58a4c24a0590edb0b.bindPopup%28popup_979fd3462dbf48bebc6ca0dde5e0b7e0%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_737444f58500434ca60dec2ee28b95a7%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7153834%2C-79.4056784%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_667857aad5b745dd88298d09d2ff65e2%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_8bfad1ea0a8c472ebe08af7d7acf3b9c%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_8bfad1ea0a8c472ebe08af7d7acf3b9c%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENorth%20Toronto%20West%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_667857aad5b745dd88298d09d2ff65e2.setContent%28html_8bfad1ea0a8c472ebe08af7d7acf3b9c%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_737444f58500434ca60dec2ee28b95a7.bindPopup%28popup_667857aad5b745dd88298d09d2ff65e2%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_029cd1c2305e43b1b88da00f0baa8c63%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7043244%2C-79.3887901%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_8b91445cd4804873b1c0cecd992e90ac%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f5d09db63e6c43d1a9f1017132c3cee2%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f5d09db63e6c43d1a9f1017132c3cee2%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EDavisville%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_8b91445cd4804873b1c0cecd992e90ac.setContent%28html_f5d09db63e6c43d1a9f1017132c3cee2%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_029cd1c2305e43b1b88da00f0baa8c63.bindPopup%28popup_8b91445cd4804873b1c0cecd992e90ac%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_20006f74c65f4094957f056054fcf087%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6895743%2C-79.3831599%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%238000ff%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%238000ff%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_2199b8dc025046e4bea0e1877b84789b%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_07aa1c9017024edcbbe2f6ef35faf070%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_07aa1c9017024edcbbe2f6ef35faf070%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EMoore%20Park%2C%20Summerhill%20East%20Cluster%201%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_2199b8dc025046e4bea0e1877b84789b.setContent%28html_07aa1c9017024edcbbe2f6ef35faf070%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_20006f74c65f4094957f056054fcf087.bindPopup%28popup_2199b8dc025046e4bea0e1877b84789b%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_f3ac6beef71346fdb4cdd8b33215e6c7%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6864123%2C-79.4000493%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d0e389e1b08744b7bd2bdc56fad75716%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e4c2d81dc17b4c6fbfd1833f5e37feca%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e4c2d81dc17b4c6fbfd1833f5e37feca%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ESummerhill%20West%2C%20Rathnelly%2C%20South%20Hill%2C%20Forest%20Hill%20SE%2C%20Deer%20Park%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d0e389e1b08744b7bd2bdc56fad75716.setContent%28html_e4c2d81dc17b4c6fbfd1833f5e37feca%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_f3ac6beef71346fdb4cdd8b33215e6c7.bindPopup%28popup_d0e389e1b08744b7bd2bdc56fad75716%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_8b9da8bae503496f847b54c8d79df4cb%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6795626%2C-79.3775294%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%238000ff%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%238000ff%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_ef7796442b3e4910bd845cbabe81fc11%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_0950a7246e9d49f7a284e56cd11699f1%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_0950a7246e9d49f7a284e56cd11699f1%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERosedale%20Cluster%201%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_ef7796442b3e4910bd845cbabe81fc11.setContent%28html_0950a7246e9d49f7a284e56cd11699f1%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_8b9da8bae503496f847b54c8d79df4cb.bindPopup%28popup_ef7796442b3e4910bd845cbabe81fc11%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c0ad7baa8f394a4592e78227f061cb1c%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.667967%2C-79.3676753%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_e735e48cca6846ada93b857e772aca92%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_172ebf5fd033444f9677424cc6f4a72c%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_172ebf5fd033444f9677424cc6f4a72c%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ESt.%20James%20Town%2C%20Cabbagetown%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_e735e48cca6846ada93b857e772aca92.setContent%28html_172ebf5fd033444f9677424cc6f4a72c%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c0ad7baa8f394a4592e78227f061cb1c.bindPopup%28popup_e735e48cca6846ada93b857e772aca92%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9365aba41191439eb4abbb6e556a0d9b%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6658599%2C-79.3831599%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_66534c83ae6649349db60fb7e5d9a212%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_b200b4b723fd41b78b7dcf3a887ae085%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_b200b4b723fd41b78b7dcf3a887ae085%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EChurch%20and%20Wellesley%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_66534c83ae6649349db60fb7e5d9a212.setContent%28html_b200b4b723fd41b78b7dcf3a887ae085%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9365aba41191439eb4abbb6e556a0d9b.bindPopup%28popup_66534c83ae6649349db60fb7e5d9a212%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_2a9d0ef8f99a498094ee4592b365c54a%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6542599%2C-79.3606359%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d02a02e5ff14497cb1c33e1475cf1c90%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_ca61f3a12df84f27b95124abb820d8aa%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_ca61f3a12df84f27b95124abb820d8aa%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERegent%20Park%2C%20Harbourfront%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d02a02e5ff14497cb1c33e1475cf1c90.setContent%28html_ca61f3a12df84f27b95124abb820d8aa%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_2a9d0ef8f99a498094ee4592b365c54a.bindPopup%28popup_d02a02e5ff14497cb1c33e1475cf1c90%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_418a07f09351407a91e1b4d35dee6b2a%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6571618%2C-79.3789371%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_335da9f4fb58450ead31fd05d50870f5%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_bbe69e7a73f14e1381b264015a095355%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_bbe69e7a73f14e1381b264015a095355%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EGarden%20District%2C%20Ryerson%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_335da9f4fb58450ead31fd05d50870f5.setContent%28html_bbe69e7a73f14e1381b264015a095355%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_418a07f09351407a91e1b4d35dee6b2a.bindPopup%28popup_335da9f4fb58450ead31fd05d50870f5%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_a3f8b2f417bd49ca8d5766874b5f9f23%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6514939%2C-79.3754179%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_e1a44d97d0fc48d29607286421ae1eb8%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_650e3b663c0b424e9e279aa9d064a0cc%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_650e3b663c0b424e9e279aa9d064a0cc%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ESt.%20James%20Town%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_e1a44d97d0fc48d29607286421ae1eb8.setContent%28html_650e3b663c0b424e9e279aa9d064a0cc%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_a3f8b2f417bd49ca8d5766874b5f9f23.bindPopup%28popup_e1a44d97d0fc48d29607286421ae1eb8%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_71bb10d31cc345c58283de54ec909b77%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6447708%2C-79.3733064%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_dfbc409f741d47a79761a21042ee0a46%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_dc6780ba6c084ccdabcbbf4848594a83%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_dc6780ba6c084ccdabcbbf4848594a83%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EBerczy%20Park%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_dfbc409f741d47a79761a21042ee0a46.setContent%28html_dc6780ba6c084ccdabcbbf4848594a83%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_71bb10d31cc345c58283de54ec909b77.bindPopup%28popup_dfbc409f741d47a79761a21042ee0a46%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_237b20c01c0049ae99f654e8b8915344%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6579524%2C-79.3873826%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_1daf050e8d9943669895940f0d7c11cb%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f9c671f7c4754b4ba318a8c2b923b997%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f9c671f7c4754b4ba318a8c2b923b997%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ECentral%20Bay%20Street%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_1daf050e8d9943669895940f0d7c11cb.setContent%28html_f9c671f7c4754b4ba318a8c2b923b997%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_237b20c01c0049ae99f654e8b8915344.bindPopup%28popup_1daf050e8d9943669895940f0d7c11cb%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_bcd4c23037654ca4ab2c6dbc9549b2bd%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6505712%2C-79.3845675%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_4151fd6b4cd249a481a84f757c64aada%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_46f2e9496a104522bc613415e17cf174%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_46f2e9496a104522bc613415e17cf174%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERichmond%2C%20Adelaide%2C%20King%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_4151fd6b4cd249a481a84f757c64aada.setContent%28html_46f2e9496a104522bc613415e17cf174%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_bcd4c23037654ca4ab2c6dbc9549b2bd.bindPopup%28popup_4151fd6b4cd249a481a84f757c64aada%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c1afca6dcfe64b3e8b939778846e81e0%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6408157%2C-79.3817523%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_21ea21ae8508434ea8ec1ff3ed7ec42d%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_00a029534a164f67a316dee72cd72480%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_00a029534a164f67a316dee72cd72480%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EHarbourfront%20East%2C%20Union%20Station%2C%20Toronto%20Islands%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_21ea21ae8508434ea8ec1ff3ed7ec42d.setContent%28html_00a029534a164f67a316dee72cd72480%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c1afca6dcfe64b3e8b939778846e81e0.bindPopup%28popup_21ea21ae8508434ea8ec1ff3ed7ec42d%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9efdd7bc7e91435d8e7df98d7d0b67f6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6471768%2C-79.3815764%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f973dfccddfd4feba1c9088c01096b04%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_9cc537256a914c0f9705bc7b114af2f4%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_9cc537256a914c0f9705bc7b114af2f4%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EToronto%20Dominion%20Centre%2C%20Design%20Exchange%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f973dfccddfd4feba1c9088c01096b04.setContent%28html_9cc537256a914c0f9705bc7b114af2f4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9efdd7bc7e91435d8e7df98d7d0b67f6.bindPopup%28popup_f973dfccddfd4feba1c9088c01096b04%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_6cc423d52db9400f8a10c029a35d7b4a%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6481985%2C-79.3798169%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_958a78b609b941f0a541ad43f0b0e1f7%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e4834c5597c5464b92a6c0efca13bd70%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e4834c5597c5464b92a6c0efca13bd70%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ECommerce%20Court%2C%20Victoria%20Hotel%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_958a78b609b941f0a541ad43f0b0e1f7.setContent%28html_e4834c5597c5464b92a6c0efca13bd70%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_6cc423d52db9400f8a10c029a35d7b4a.bindPopup%28popup_958a78b609b941f0a541ad43f0b0e1f7%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c7c647dc5204476a960e63889c995250%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.7116948%2C-79.4169356%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2300b5eb%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2300b5eb%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_62fc10db2f434aba93caf13ed2f116a3%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_64db1ef569004e53b7f7ccc57b36a3bf%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_64db1ef569004e53b7f7ccc57b36a3bf%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERoselawn%20Cluster%202%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_62fc10db2f434aba93caf13ed2f116a3.setContent%28html_64db1ef569004e53b7f7ccc57b36a3bf%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c7c647dc5204476a960e63889c995250.bindPopup%28popup_62fc10db2f434aba93caf13ed2f116a3%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b67f83c4286b420c9fe2d3f888a18f04%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6969476%2C-79.4113072%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c0911ac6e3e242bd8aae4172591916f9%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e7d14d29a0714851b81bb4e5dac229a3%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e7d14d29a0714851b81bb4e5dac229a3%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EForest%20Hill%20North%20%26amp%3B%20West%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c0911ac6e3e242bd8aae4172591916f9.setContent%28html_e7d14d29a0714851b81bb4e5dac229a3%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b67f83c4286b420c9fe2d3f888a18f04.bindPopup%28popup_c0911ac6e3e242bd8aae4172591916f9%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_be49e58ddbcc44f7a29d3c27823b4b87%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6727097%2C-79.4056784%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c0cc273da7084c8a86c2ff85a4684456%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_ac5100f72c334495b1a4555da607eb15%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_ac5100f72c334495b1a4555da607eb15%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EThe%20Annex%2C%20North%20Midtown%2C%20Yorkville%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c0cc273da7084c8a86c2ff85a4684456.setContent%28html_ac5100f72c334495b1a4555da607eb15%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_be49e58ddbcc44f7a29d3c27823b4b87.bindPopup%28popup_c0cc273da7084c8a86c2ff85a4684456%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_37c6da27672e4b15afecba8ca00a8cf5%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6626956%2C-79.4000493%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_83b14e9ab6064ac3b97a21bb2a87e89e%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_860fd6885ea140fb9e8f60f774d76ccc%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_860fd6885ea140fb9e8f60f774d76ccc%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EUniversity%20of%20Toronto%2C%20Harbord%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_83b14e9ab6064ac3b97a21bb2a87e89e.setContent%28html_860fd6885ea140fb9e8f60f774d76ccc%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_37c6da27672e4b15afecba8ca00a8cf5.bindPopup%28popup_83b14e9ab6064ac3b97a21bb2a87e89e%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_2c2323405e0340c5aa53cea8722ad616%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6532057%2C-79.4000493%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_8c364c284ba04c09b268d8246901e6ac%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_1517de93876d4b86a5b065800b0f4401%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_1517de93876d4b86a5b065800b0f4401%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EKensington%20Market%2C%20Chinatown%2C%20Grange%20Park%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_8c364c284ba04c09b268d8246901e6ac.setContent%28html_1517de93876d4b86a5b065800b0f4401%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_2c2323405e0340c5aa53cea8722ad616.bindPopup%28popup_8c364c284ba04c09b268d8246901e6ac%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c8ef3c673fdc46efb3ed7f7e9292f6d1%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6289467%2C-79.3944199%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_3fc1dc3f10c242309aaab5e1099754ff%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_a243efea7f9a44388d1e3dfe40a01aa7%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_a243efea7f9a44388d1e3dfe40a01aa7%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ECN%20Tower%2C%20King%20and%20Spadina%2C%20Railway%20Lands%2C%20Harbourfront%20West%2C%20Bathurst%20Quay%2C%20South%20Niagara%2C%20Island%20airport%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_3fc1dc3f10c242309aaab5e1099754ff.setContent%28html_a243efea7f9a44388d1e3dfe40a01aa7%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c8ef3c673fdc46efb3ed7f7e9292f6d1.bindPopup%28popup_3fc1dc3f10c242309aaab5e1099754ff%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_136292f4d06f428baf0deaea68369b7e%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6464352%2C-79.374846%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d29a1f28863d411c851f6f3870790a30%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_71561b31d7a64144b25ffdb28048bdff%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_71561b31d7a64144b25ffdb28048bdff%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EEnclave%20of%20M5E%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d29a1f28863d411c851f6f3870790a30.setContent%28html_71561b31d7a64144b25ffdb28048bdff%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_136292f4d06f428baf0deaea68369b7e.bindPopup%28popup_d29a1f28863d411c851f6f3870790a30%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_d7676dc660b6465498aa1c5f396bba8c%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6484292%2C-79.3822802%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_4e00152957cf45ccb5d355c16f764ada%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e4ab220a954141e09a24c49ebfdbbe9a%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e4ab220a954141e09a24c49ebfdbbe9a%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EFirst%20Canadian%20Place%2C%20Underground%20city%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_4e00152957cf45ccb5d355c16f764ada.setContent%28html_e4ab220a954141e09a24c49ebfdbbe9a%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_d7676dc660b6465498aa1c5f396bba8c.bindPopup%28popup_4e00152957cf45ccb5d355c16f764ada%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_d288256ed4e94fd5ba7324f112632781%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.669542%2C-79.4225637%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f77cd0bb6f9c4e8cb5352ced09614fda%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_6e002963b4c44bc6b9595d8826cc8593%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_6e002963b4c44bc6b9595d8826cc8593%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EChristie%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f77cd0bb6f9c4e8cb5352ced09614fda.setContent%28html_6e002963b4c44bc6b9595d8826cc8593%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_d288256ed4e94fd5ba7324f112632781.bindPopup%28popup_f77cd0bb6f9c4e8cb5352ced09614fda%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_038419f6d122405083e53bd590aae067%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6690051%2C-79.4422593%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_0ff7da9634e4455ba04a1512f439a57d%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_9c5bd536530a4ca28f6bd4eef3a5b5cd%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_9c5bd536530a4ca28f6bd4eef3a5b5cd%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EDufferin%2C%20Dovercourt%20Village%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_0ff7da9634e4455ba04a1512f439a57d.setContent%28html_9c5bd536530a4ca28f6bd4eef3a5b5cd%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_038419f6d122405083e53bd590aae067.bindPopup%28popup_0ff7da9634e4455ba04a1512f439a57d%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b005ccfebed74ec780d218dc7e0e33a8%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6479267%2C-79.4197497%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c69ebc87d204408c91928778c9fad4da%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_7035634643024a988ef276a32e1c5099%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_7035634643024a988ef276a32e1c5099%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ELittle%20Portugal%2C%20Trinity%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c69ebc87d204408c91928778c9fad4da.setContent%28html_7035634643024a988ef276a32e1c5099%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b005ccfebed74ec780d218dc7e0e33a8.bindPopup%28popup_c69ebc87d204408c91928778c9fad4da%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_724f09b9f3c74b44b46be1311ce494dd%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6368472%2C-79.4281914%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_95a05b90611a48419e007a53513458d3%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_a2847b7f573c4d9fbd8dfbc33fa7cee2%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_a2847b7f573c4d9fbd8dfbc33fa7cee2%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EBrockton%2C%20Parkdale%20Village%2C%20Exhibition%20Place%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_95a05b90611a48419e007a53513458d3.setContent%28html_a2847b7f573c4d9fbd8dfbc33fa7cee2%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_724f09b9f3c74b44b46be1311ce494dd.bindPopup%28popup_95a05b90611a48419e007a53513458d3%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_9023dff3b7964331ba64c52ba558bdd4%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6616083%2C-79.4647633%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_1a6cb14908a84cec8f5261c61331b8ff%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_480d7f5da23a4f34956f340593c4e5e0%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_480d7f5da23a4f34956f340593c4e5e0%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EHigh%20Park%2C%20The%20Junction%20South%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_1a6cb14908a84cec8f5261c61331b8ff.setContent%28html_480d7f5da23a4f34956f340593c4e5e0%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_9023dff3b7964331ba64c52ba558bdd4.bindPopup%28popup_1a6cb14908a84cec8f5261c61331b8ff%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_7bbb96730609443fb4f1733ac936030f%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6489597%2C-79.456325%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_cbb6eb90e35b4efca91d42952222f3fb%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f48464d1195749aeafdf30322d6df5d6%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f48464d1195749aeafdf30322d6df5d6%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EParkdale%2C%20Roncesvalles%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_cbb6eb90e35b4efca91d42952222f3fb.setContent%28html_f48464d1195749aeafdf30322d6df5d6%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_7bbb96730609443fb4f1733ac936030f.bindPopup%28popup_cbb6eb90e35b4efca91d42952222f3fb%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_84123c5681e24b61a96a11bbf3c53513%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6515706%2C-79.4844499%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f61b8d1e4aa441bfac53cc2182b74899%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_636bf4bc585d47f0bd49532efd5af69a%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_636bf4bc585d47f0bd49532efd5af69a%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERunnymede%2C%20Swansea%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f61b8d1e4aa441bfac53cc2182b74899.setContent%28html_636bf4bc585d47f0bd49532efd5af69a%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_84123c5681e24b61a96a11bbf3c53513.bindPopup%28popup_f61b8d1e4aa441bfac53cc2182b74899%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_eb3dffb037c24bc49e81453a29e4814c%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B43.6627439%2C-79.321558%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22%2380ffb4%22%2C%0A%20%20%22fillOpacity%22%3A%200.7%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_21544e3e59dd4fa784bc97ff8b45fc33%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_6f4ae038472444a1b37d8fa8e5e72481%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_b3c981945edb442f8a3519594783251c%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_b3c981945edb442f8a3519594783251c%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EEnclave%20of%20M4L%20Cluster%203%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_6f4ae038472444a1b37d8fa8e5e72481.setContent%28html_b3c981945edb442f8a3519594783251c%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_eb3dffb037c24bc49e81453a29e4814c.bindPopup%28popup_6f4ae038472444a1b37d8fa8e5e72481%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%3C/script%3E onload="this.contentDocument.open();this.contentDocument.write(    decodeURIComponent(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



It seems like a lot of the neighborhoods fall into one cluster, the green dots. Further analysis might reveal why this is.
