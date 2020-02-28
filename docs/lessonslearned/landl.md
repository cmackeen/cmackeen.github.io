---
layout: default
title: Lessons Learned
nav_order: 5
has_children: true
permalink: /docs/lessonslearned
---

# Lessons Learned
{: .no_toc }

As I look to share succinct content on my Projects page, I would also think it helpful to catalog the unseen obstacles, and how I stumbled through them.


1. TOC
{:toc}

## Employing pyspark, scikit-learn and audio signal processing to cluster unevenly sample EXAFS spectra
2/25/2020
{: .fw-300 }


{%include hexafs_preprocess_v2.html %}

 Clustering:

{%include ML_dbscan_mfcc.html %}

## Babylonjs Interactive Spheres
babylonjs playground makes it easy to rnder interactive 3D scenes and download it as injectable html.

First, I hade to generate hexagonal close packed coordinates:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from consts import *


'''
we're using  python3 for this . . 
'''
num=3
rad=1

aa=hcp_coords(num, rad)
jstext=open("babylonjs_inject.js", 'a')

jstext.write(" // This html was created for the babylonjs playground  \n")
print(aa)

for i in range(len(aa)-2):
	jstext.write("var sphere" + str(i)+ "  = BABYLON.MeshBuilder.CreateSphere(\"sphere \", {diameter: 2, segments: 32}, scene);   \n")
	


jstext.write("\n \n //Moving the speheres' origins")

for i in range(len(aa)-2):
	jstext.write("sphere"+str(i)+".position.x =" +str(aa[i][0]) + "\n")
	jstext.write("sphere"+str(i)+".position.y =" +str(aa[i][1]) + "\n")
	jstext.write("sphere"+str(i)+".position.z =" +str(aa[i][2]) + "\n")

jstext.close()
```

...  MORE on its way ....

## Bokeh 3D interactive iframe via inline visjs Graph3D

I wanted a smooth interactive 3D mesh plot on the gitpage of dirigible payload as a function of volume and altitude. This proved tricky, and because the site is static the normal bokeh 3D template in the gallery did not work. Further, when I finally stapled together this bokeh plot with inline javascript TS code, jekyll did not include it with simple injected html via liquid tags from the _includes directory. I needed to render the 3D plot to html, change the axis labels and title in that html, and embed it as an iframe from the /assets/ directory. Alas we get [this plot](/projects/docs/supratmos/##Payload-Analysis-round-1); kinda informative, but more pretty!

Code is below, and this review will be updated shortly.


```python
from __future__ import division

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from consts import *
from atmos_prop import *
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D



from bokeh.core.properties import Instance, String
from bokeh.models import ColumnDataSource, LayoutDOM
from bokeh.io import show
from bokeh.util.compiler import TypeScript
from bokeh.plotting import output_file
from bokeh.layouts import column

#Conditions that will alter surface plot

alt_max=13000

volrange=[6000,20000]
volmin=volrange[0]
volmax=volrange[1]

foamrho_in=25
foamthick_in=.06

####################

atm=atmosphere()
sea_level_atm=101325
x=np.linspace(0,alt_max,31)
pressure=[]
temp=[]
airrho_var=[]
h2rho_var=[]



class dirble:
	def __init__(self):
		print('brrrrrhhhkpkpkpzzzznnnn  .  .  .   starting dirigible')

		 
	def payload(self,balvolmax, balnum, foamthick, foamrho, h2rho,airrho,gasvol):
		foam_vol=sphr(invsphr(balvolmax)+2*foamthick)[0] - balvolmax
		if gasvol>balvolmax:
			m_gasnet=balvolmax*(airrho -h2rho)
		else:
			m_gasnet=gasvol*(airrho -h2rho)
		mylrho=1390
		m_mylar=0.000127*sphr(invsphr(balvolmax))[1]*mylrho
		m_net=balnum*(m_gasnet - (m_mylar+foam_vol*foamrho))
		
		pload=m_net
		return pload,m_gasnet


def dyn_volrat(pressure, temp):
	return 1.*temp/pressure*(sea_level_atm/268.15)

vol_rat=[]
for i in x:
	pressure.append(atm.piecewise_atmos(i)[0])
	temp.append(atm.piecewise_atmos(i)[1])
	airrho_var.append(alt_air_rho(atm.piecewise_atmos(i)[0],atm.piecewise_atmos(i)[1]))

	h2rho_var.append(2*(atm.piecewise_atmos(i)[0]*.00100784)/(8.314*(atm.piecewise_atmos(i)[1]+273.15)))
	vol_rat.append( dyn_volrat( atm.piecewise_atmos (i) [0],atm.piecewise_atmos(i)[1]+273.15))

def dynvol(pressure,temp,init_v):
	return init_v* 1.*temp/pressure*(sea_level_atm/268.15)

skytanic=dirble()
payload_2d=[]
payload_fixedvol=[]
mgasnet=[]
#maxvol=200000

vol_space=np.linspace(volmin,volmax,31)
init_volfrac=0.4

for vol_ix,maxvol in zip(range(len(vol_space)), vol_space):
	payload=[]
	for alt_ix, i in zip(range(len(x)), x):
		airrho_var=alt_air_rho(atm.piecewise_atmos(i)[0],atm.piecewise_atmos(i)[1])
		h2rho_var=2*(atm.piecewise_atmos(i)[0]*.00100784)/(8.314*(atm.piecewise_atmos(i)[1]+273.15))
		payload.append(  skytanic.payload(maxvol,1,foamthick_in,foamrho_in,h2rho_var,airrho_var, dynvol(atm.piecewise_atmos (i) [0],atm.piecewise_atmos(i)[1]+273.15,init_volfrac*maxvol))[0])
		#payload_fixedvol.append(  skytanic.payload(maxvol,1,.05,60,h2rho_var[alt_ix],airrho_var[alt_ix], dynvol(atm.piecewise_atmos (i) [0],atm.piecewise_atmos(i)[1]+273.15,1*maxvol))[0])
		#mgasnet.append(skytanic.payload(maxvol,1,.05,60,h2rho_var[alt_ix],airrho_var[alt_ix],dynvol(atm.piecewise_atmos (i) [0],atm.piecewise_atmos(i)[1]+273.15,init_volfrac*maxvol))[1])
	payload_2d.append(payload)

'''
def functionz(vol,alt):
		airrho_var=alt_air_rho(atm.piecewise_atmos(alt)[0],atm.piecewise_atmos(alt)[1])
		h2rho_var=2*(atm.piecewise_atmos(alt)[0]*.00100784)/(8.314*(atm.piecewise_atmos(alt)[1]+273.15))
		return skytanic.payload(vol,1,.05,60,h2rho_var,airrho_var, dynvol(atm.piecewise_atmos (alt) [0],atm.piecewise_atmos(alt)[1]+273.15,init_volfrac*vol))[0]

'''


###### .ts wrapped stuff



##############################################
##############################################


TS_CODE = """
// This custom model wraps one part of the third-party vis.js library:
//
//     http://visjs.org/index.html
//
// Making it easy to hook up python data analytics tools (NumPy, SciPy,
// Pandas, etc.) to web presentations using the Bokeh server.

import {LayoutDOM, LayoutDOMView} from "models/layouts/layout_dom"
import {ColumnDataSource} from "models/sources/column_data_source"
import {LayoutItem} from "core/layout"
import * as p from "core/properties"

declare namespace vis {
  class Graph3d {
    constructor(el: HTMLElement, data: object, OPTIONS: object)
    setData(data: vis.DataSet): void
  }

  class DataSet {
    add(data: unknown): void
  }
}

// This defines some default options for the Graph3d feature of vis.js
// See: http://visjs.org/graph3d_examples.html for more details.
const OPTIONS = {
  width: '900px',
  height: '900px',
  showPerspective: true,
  showGrid: true,
  keepAspectRatio: true,
  verticalRatio: 1.0,
  legendLabel: 'stuff',
  cameraPosition: {
    horizontal: -0.35,
    vertical: 0.22,
    distance: 1.8,
  },
}


// To create custom model extensions that will render on to the HTML canvas
// or into the DOM, we must create a View subclass for the model.
//
// In this case we will subclass from the existing BokehJS ``LayoutDOMView``
export class Surface3dView extends LayoutDOMView {
  model: Surface3d

  private _graph: vis.Graph3d

  initialize(): void {
    super.initialize()

    const url = "https://cdnjs.cloudflare.com/ajax/libs/vis/4.16.1/vis.min.js"
    const script = document.createElement("script")
    script.onload = () => this._init()
    script.async = false
    script.src = url
    document.head.appendChild(script)
  }

  private _init(): void {
    // Create a new Graph3s using the vis.js API. This assumes the vis.js has
    // already been loaded (e.g. in a custom app template). In the future Bokeh
    // models will be able to specify and load external scripts automatically.
    //
    // BokehJS Views create <div> elements by default, accessible as this.el.
    // Many Bokeh views ignore this default <div>, and instead do things like
    // draw to the HTML canvas. In this case though, we use the <div> to attach
    // a Graph3d to the DOM.
    this._graph = new vis.Graph3d(this.el, this.get_data(), OPTIONS)

    // Set a listener so that when the Bokeh data source has a change
    // event, we can process the new data
    this.connect(this.model.data_source.change, () => {
      this._graph.setData(this.get_data())
    })
  }

  // This is the callback executed when the Bokeh data has an change. Its basic
  // function is to adapt the Bokeh data source to the vis.js DataSet format.
  get_data(): vis.DataSet {
    const data = new vis.DataSet()
    const source = this.model.data_source
    for (let i = 0; i < source.get_length()!; i++) {
      data.add({
        x: source.data[this.model.x][i],
        y: source.data[this.model.y][i],
        z: source.data[this.model.z][i],
      })
    }
    return data
  }

  get child_models(): LayoutDOM[] {
    return []
  }

  _update_layout(): void {
    this.layout = new LayoutItem()
    this.layout.set_sizing(this.box_sizing())
  }
}

// We must also create a corresponding JavaScript BokehJS model subclass to
// correspond to the python Bokeh model subclass. In this case, since we want
// an element that can position itself in the DOM according to a Bokeh layout,
// we subclass from ``LayoutDOM``
export namespace Surface3d {
  export type Attrs = p.AttrsOf<Props>

  export type Props = LayoutDOM.Props & {
    x: p.Property<string>
    y: p.Property<string>
    z: p.Property<string>
    data_source: p.Property<ColumnDataSource>
  }
}

export interface Surface3d extends Surface3d.Attrs {}

export class Surface3d extends LayoutDOM {
  properties: Surface3d.Props

  constructor(attrs?: Partial<Surface3d.Attrs>) {
    super(attrs)
  }

  // The ``__name__`` class attribute should generally match exactly the name
  // of the corresponding Python class. Note that if using TypeScript, this
  // will be automatically filled in during compilation, so except in some
  // special cases, this shouldn't be generally included manually, to avoid
  // typos, which would prohibit serialization/deserialization of this model.
  static __name__ = "Surface3d"

  static initClass() {
    // This is usually boilerplate. In some cases there may not be a view.
    this.prototype.default_view = Surface3dView

    // The @define block adds corresponding "properties" to the JS model. These
    // should basically line up 1-1 with the Python model class. Most property
    // types have counterparts, e.g. ``bokeh.core.properties.String`` will be
    // ``p.String`` in the JS implementatin. Where the JS type system is not yet
    // as rich, you can use ``p.Any`` as a "wildcard" property type.
    this.define<Surface3d.Props>({
      x:            [ p.String  ],
      y:            [ p.String   ],
      z:            [ p.String   ],
      data_source:  [ p.Instance ],
    })
  }
}
Surface3d.initClass()
"""

# This custom extension model will have a DOM view that should layout-able in
# Bokeh layouts, so use ``LayoutDOM`` as the base class. If you wanted to create
# a custom tool, you could inherit from ``Tool``, or from ``Glyph`` if you
# wanted to create a custom glyph, etc.
class Surface3d(LayoutDOM):

    # The special class attribute ``__implementation__`` should contain a string
    # of JavaScript (or CoffeeScript) code that implements the JavaScript side
    # of the custom extension model.
    __implementation__ = TypeScript(TS_CODE)

    # Below are all the "properties" for this model. Bokeh properties are
    # class attributes that define the fields (and their types) that can be
    # communicated automatically between Python and the browser. Properties
    # also support type validation. More information about properties in
    # can be found here:
    #
    #    https://bokeh.pydata.org/en/latest/docs/reference/core.html#bokeh-core-properties

    # This is a Bokeh ColumnDataSource that can be updated in the Bokeh
    # server by Python code
    data_source = Instance(ColumnDataSource)

    # The vis.js library that we are wrapping expects data for x, y, and z.
    # The data will actually be stored in the ColumnDataSource, but these
    # properties let us specify the *name* of the column that should be
    # used for each field.
    x = String

    y = String

    z = String



##############################################
##############################################
output_file("pload_surface_pt4vfrac.html")


#######plotting portion

surfdata=np.array(payload_2d)

length = surfdata.shape[0]
width = surfdata.shape[1]

zeroplane=np.zeros((length,width))
X, Y= np.meshgrid(np.linspace(volmin,volmax,length), np.linspace(0,alt_max,width))


source = ColumnDataSource(data=dict(x=X, y=Y, z=np.transpose(surfdata)))
sourcezero = ColumnDataSource(data=dict(x=X, y=Y, z=np.transpose(zeroplane)))


surface = Surface3d(x="x", y="y", z="z", data_source=source)
Surface3d(x="x", y="y", z="z", data_source=sourcezero)


layout=column(surface)
show(layout)

```


## Bokeh Sliders for Interactive Plot
coming soon ... 


## Argparse and Commandline bash executed python tools

The development of a useful and accesible tool to remove oscillations in the
pre-edge area of x-ray absorption data from [this post](../projects/prestencil/)
included command line functionality. This functionality was derived from code provided on [omgenomics
post](http://omgenomics.com/python-command-line-program/) that shows a quick and easy template for argparse usage in python. My code snippet below shows a couple added things, and the "937" was used as a unique numeric switch. The short github for this tool is [here](https://github.com/cmackeen/exafsbk).


```python
def main():
    parser=argparse.ArgumentParser(description="Takes input 2 column (tab sep) list, expicitly titled 'stencdat.inp'. Left column is data files to be corrected, Right column is simulated ks 'stencil' files. Cheers  ")
    parser.add_argument("-es",help="Energy of the stencil's edge [eV] (the lower energy edge tat bleeds into edge of interest)" ,dest="e_stenc", type=float, required=True)
    parser.add_argument("-e1",help="Edge of interest [eV] (Default=auto from max edge slope)" ,dest="e1", type=float, default=37)
    parser.add_argument("-amp",help="Manually fix scaling amplitude of stencil" ,dest="ampfix", type=float, default=937)
    parser.add_argument("-eshift",help="Manually fix energy shift of stencil" ,dest="eshfix", type=float, default=937)
    parser.add_argument("-off",help="Use when pre-edge does not seem to oscillate around 0 (Default is no constant offset)" ,dest="offset_bool",action="store_true",default=False)
    parser.set_defaults(func=run)
    args=parser.parse_args()
    args.func(args)


#-----------------------------------------------------------------------------------------------



# set number of rows of header (default 14):
header_length = 14
# length to cut data to
cut_length = 25
cut1_length = 125
# delete temp header file and data file after cominbed?
cleanup = bool(True)

def run(args):

    E_stenc = args.e_stenc # these match the "dest": dest="input"
    E1 = args.e1 # from dest="output"
    file_name='stencdat.inp'
    offset=args.offset_bool
    
    #E1=22121.5
#e_stenc=21761.5

```

## Bokeh Periodic Table

5/13/2019 
{: .fw-300 } 


So when I dove in and configured this Jekyll template based website,
I had a lot of quick css learning to do. I am inexperienced with
current Jekyll template schema, and where everything is, and that
important directories start with underscores. I was focused on a
clean and timeless look, so I wanted to hold this core theme with
how I approach generating data visuals. The first example can been
seen in [this
post](../projects/hexafs/#a-first-look-at-historical-exafs).


Bokeh was the answer! Maybe I am riding with some recency bias, but Bokeh is
straightforward, good-lookin', and works in both R and python. Templates are
widely available (and specific); and further there seems to be active community
memebers that can share their plots. I found a decent template of a periodic
table with on the [Bokeh
site](https://bokeh.pydata.org/en/latest/docs/gallery/periodic.html), but the
lanthanides were not included. This quickly spurred me to search for a Bokeh
plot with the lanthanides, and I stumbled upon [Andrew Rosen's
project](https://github.com/arosen93/ptable_trends), which even included color
maps determined by element specific data. A cursory search can save you hours !

What I found editing Bokeh templates is that things just tend to work. This is an
effect of using python, sure, but when I had to customize the font color of the
hover-menu I actually could do it with just a raw html div inside the python
code. So the most trying obstacle was the hovertool custom format, not bad.
Code I used is below:

``` python
from __future__ import absolute_import
from bokeh.models import (ColumnDataSource, LinearColorMapper, LogColorMapper, 
	ColorBar, BasicTicker, HoverTool)
from bokeh.plotting import figure
from bokeh.io import output_file, show
from bokeh.sampledata.periodic_table import elements
from bokeh.transform import dodge
from csv import reader
from matplotlib.colors import Normalize, LogNorm, to_hex
from matplotlib.cm import plasma, inferno, magma, viridis, ScalarMappable
from pandas import options
import argparse
options.mode.chained_assignment = None

output_file('ptable_trends.html')

#Parse arguments
parser = argparse.ArgumentParser(description='Plot periodic trends as a heat '
	'map over the periodic table of elements')
parser.add_argument('filename',type=str,help='Filename (with extension) of '
	'CSV-formatted data')
parser.add_argument('--width',type=int,default=1050,help='Width (in pixels) of '
	'figure')
parser.add_argument('--cmap_choice',type=int,default=0,choices=range(0,4),
	help='Color palette choice: 0 = Plasma, 1 = Inferno, 2 = Magma, 3 = Viridis')
parser.add_argument('--alpha',type=float,default=0.65,help='Alpha value '
	'for color scale (ranges from 0 to 1)')
parser.add_argument('--extended',default='True',choices=["False","false","True","true"],
	help='Keyword for excluding (false) or including (true) the lanthanides and actinides')
parser.add_argument('--period_remove',type=str,nargs='*',help='Period(s) to remove')
parser.add_argument('--group_remove',type=str,nargs='*',help='Group(s) to remove')
parser.add_argument('--log_scale',type=int,default=0,choices=range(0,2),
	help='Keyword for linear (0) or logarithmic (1) color bar')
parser.add_argument('--cbar_height',type=int,help='Height (in pixels) of color '
	'bar')
parser.add_argument('--cbar_standoff',type=int,help='Distance (in pixels) that the '
	'colorbar tick values should be from the colorbar itself')
parser.add_argument('--cbar_fontsize',type=int,help='Fontsize (in pt) that the '
	'colorbar tick values should be')

args = parser.parse_args()
filename = args.filename
width = args.width
cmap_choice = args.cmap_choice
alpha = args.alpha
if args.extended.lower() == 'true':
	extended = True
elif args.extended.lower() == 'false':
	extended = False
else:
	raise ValueError('Invalid keyword for --extended')
log_scale = args.log_scale
cbar_height = args.cbar_height
cbar_standoff = args.cbar_standoff
cbar_fontsize = args.cbar_fontsize
period_remove = args.period_remove
group_remove = args.group_remove

if not cbar_standoff:
	cbar_standoff = 12
if not cbar_fontsize:
	cbar_fontsize = 12

#Error handling
if width < 0:
	raise argparse.ArgumentTypeError('--width must be a positive integer')
if alpha < 0 or alpha > 1:
	raise argparse.ArgumentTypeError('--alpha must be between 0 and 1')
if cbar_height is not None and cbar_height < 0:
	raise argparse.ArgumentTypeError('--cbar_height must be a positive integer')

#Assign color palette based on input argument
if cmap_choice == 0:
	cmap = plasma
	bokeh_palette = 'Plasma256'
elif cmap_choice == 1:
	cmap = inferno
	bokeh_palette = 'Inferno256'
elif cmap_choice == 2:
	cmap = magma
	bokeh_palette = 'Magma256'
elif cmap_choice == 3:
	cmap = viridis
	bokeh_palette = 'Viridis256'
	
#Define number of and groups
period_label = ['1', '2', '3', '4', '5', '6', '7']
group_range = [str(x) for x in range(1, 19)]

#Remove any groups or periods
if group_remove:
	for gr in group_remove:
		gr = gr.strip()
		group_range.remove(gr)
if period_remove:
	for pr in period_remove:
		pr = pr.strip()
		period_label.remove(pr)

#Read in data from CSV file
data_elements = []
data_list = []
for row in reader(open(filename)):
	data_elements.append(row[0])
	data_list.append(row[1])
data = [float(i) for i in data_list]

if len(data) != len(data_elements):
	raise ValueError('Unequal number of atomic elements and data points')

period_label.append('blank')
period_label.append('La')
period_label.append('Ac')

if extended:
	count = 0
	for i in range(56,70):
	    elements.period[i] = 'La'
	    elements.group[i] = str(count+4)
	    count += 1

	count = 0
	for i in range(88,102):
	    elements.period[i] = 'Ac'
	    elements.group[i] = str(count+4)
	    count += 1

#Define matplotlib and bokeh color map
if log_scale == 0:
	color_mapper = LinearColorMapper(palette = bokeh_palette, low=min(data), 
		high=max(data))
	norm = Normalize(vmin = min(data), vmax = max(data))
elif log_scale == 1:
	for datum in data:
		if datum < 0:
			raise ValueError('Entry for element '+datum+' is negative but'
			' log-scale is selected')
	color_mapper = LogColorMapper(palette = bokeh_palette, low=min(data), 
		high=max(data))
	norm = LogNorm(vmin = min(data), vmax = max(data))
color_scale = ScalarMappable(norm=norm, cmap=cmap).to_rgba(data,alpha=None)

#Define color for blank entries
blank_color = '#c4c4c4'
color_list = []
count_list=[]
for i in range(len(elements)):
	color_list.append(blank_color)
	count_list.append(0)

#Compare elements in dataset with elements in periodic table
for i, data_element in enumerate(data_elements):
	element_entry = elements.symbol[elements.symbol.str.lower() == data_element.lower()]
	if element_entry.empty == False:
		element_index = element_entry.index[0]
	else:
		print('WARNING: Invalid chemical symbol: '+data_element)
	if color_list[element_index] != blank_color:
		print('WARNING: Multiple entries for element '+data_element)
	color_list[element_index] = to_hex(color_scale[i])
	count_list[element_index] = data[i]

#Define figure properties for visualizing data
source = ColumnDataSource(
    data=dict(
        group=[str(x) for x in elements['group']],
        period=[str(y) for y in elements['period']],
        sym=elements['symbol'],
        atomic_number=elements['atomic number'],
        type_color=color_list,
        countsraw=count_list,
    )
)

#Plot the periodic table
p = figure(x_range=group_range, y_range=list(reversed(period_label)),
	tools='hover,save')
p.plot_width = width
p.outline_line_color = None
p.toolbar_location='above'
p.rect('group', 'period', 0.9, 0.9, source=source,
       alpha=alpha, color='type_color')
p.axis.visible = False
text_props = {
    'source': source,
    'angle': 0,
    'color': 'black',
    'text_align': 'left',
    'text_baseline': 'middle'
}
x = dodge("group", -0.4, range=p.x_range)
y = dodge("period", 0.3, range=p.y_range)
p.text(x=x, y='period', text='sym',
       text_font_style='bold', text_font_size='16pt', **text_props)
p.text(x=x, y=y, text='atomic_number',
       text_font_size='11pt', **text_props)



color_bar = ColorBar(color_mapper=color_mapper,
	ticker=BasicTicker(desired_num_ticks=10),border_line_color=None,
	label_standoff=cbar_standoff,location=(0,0),orientation='vertical',
    scale_alpha=alpha,major_label_text_font_size=str(cbar_fontsize)+'pt')

if cbar_height is not None:
	color_bar.height = cbar_height

p.add_layout(color_bar,'right')
p.grid.grid_line_color = None


# Making the custom format of the hover tooltips, using html
p.select_one(HoverTool).tooltips = """
    <font face="Arial" size="5"<bf>@sym</bf>
    count = @countsraw </font>
"""




show(p)


```

 

