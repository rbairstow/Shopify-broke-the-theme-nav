# Vantage Navigation patch
On 17th August Shopify started Beta testing a new navigation setup
Beta is not actual, when it doesn't work as expected however theme developers are always asked to fix it

The following code change will work with sectioned themes only which were introduced in early 2017. 

****** IF YOUR THEME DOES NOT HAVE SECTION EDITING CAPABILITY YOU WILL NEED TO USE THE LEGACY SETUP FOR THIS ******

VANTAGE THEME ONLY:   Themes > Actions > Edit HTML / CSS > Snippets > navigation.liquid

```html

Replace all of this code:

<ul class="nav">
  {% for link in linklists[section.settings.main_nav ].links %}
  {% capture link_handle %}{{ link.title | handle }}{% endcapture %}
  {% capture linkurl %}{{ link.url }}{% endcapture %} 
  {% capture child_list_handle %}{{ link.title | handleize }}{% endcapture %}          
  {% if linklists[child_list_handle] and linklists[child_list_handle].links.size > 0 %}
  <li class="dropdown">{{ link.title | link_to: link.url }}  
    <ul class="submenu">
      {% for l in linklists[child_list_handle].links %}
      {% capture child_list_handle %}{{ l.title | handleize }}{% endcapture %}
      {% if linklists[child_list_handle] and linklists[child_list_handle].links.size > 0 %}      
      <li class="nest"><a href="{{ l.url }}">{{ l.title }}</a>                 
        <ul class="nested">
          {% for l in linklists[child_list_handle].links %}
          <li><a href="{{ l.url }}">{{ l.title }}</a></li>
          {% endfor %}
        </ul>
      </li>
      {% else %}
      <li><a href="{{ l.url }}">{{ l.title }}</a></li>    
      {% endif %}
      {% endfor %}
    </ul>
  </li>
  {% else %}
  <li>{{ link.title | link_to: link.url }}</li>
  {% endif %}     
  {% endfor %}
</ul>

with this:

<ul class="nav">
  {% for link in linklists[section.settings.main_nav ].links %}         
  {% if linklists[link.handle] and linklists[link.handle].links.size > 0 %}
  <li class="dropdown">{{ link.title | link_to: link.url }}  
    <ul class="submenu">
      {% for l in linklists[link.handle].links %}
      {% if linklists[l.handle] and linklists[l.handle].links.size > 0 %}      
      <li class="nest"><a href="{{ l.url }}">{{ l.title }}</a>                 
        <ul class="nested">
          {% for l in linklists[l.handle].links %}
          <li><a href="{{ l.url }}">{{ l.title }}</a></li>
          {% endfor %}
        </ul>
      </li>
      {% else %}
      <li><a href="{{ l.url }}">{{ l.title }}</a></li>    
      {% endif %}
      {% endfor %}
    </ul>
  </li>
  {% else %}
  <li>{{ link.title | link_to: link.url }}</li>
  {% endif %}     
  {% endfor %}
</ul>
```

Click save when done.
