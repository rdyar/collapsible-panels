---
layout: default
---


{% for item in site.data.faqs %}
<div class="panel">
<div class="panel-heading" data-toggle="{{forloop.index}}"> 	
<img class="panel-heading-question" data-toggle="{{forloop.index}}" src="questionmark.png"><a data-toggle="{{forloop.index}}" href="#">{{item.title}}</a>
</div>
<div class="panel-body hidden-element" data-body="{{forloop.index}}"> 
{{item.body}} 
</div>
</div>
{% endfor %}

# How it works

Each panel is made up of a panel div which contains a panel-heading div for the title, and then a panel-body div for the body.

Inside the panel-heading is the question mark image, and the title inside an a tag. The panel-heading div, the img and the a all have a data-toggle set to an index number - for any given panel all three items have the same data-toggle number. The img and the a tags need the same data-toggle or they won't expand the body if you don't click directly on them.

When you click on a panels heading/title, the javascript code looks for a body that matches the same data-toggle number - the body has a data-body index number that is the same index number as the panel heading that it goes with. 

Also when you click on a given heading, the code goes thru all other bodies and collapses them - so that as you expand one panel, any other open panel will collapse.

## Example of the html to get the titles and bodies from a data file with Jekyll:
```
{% raw %}
	{% for item in site.data.faqs %}
	<div class="panel">
		<div class="panel-heading" data-toggle="{{forloop.index}}"> 	
			<img class="panel-heading-question" data-toggle="{{forloop.index}}" src="questionmark.png">  
			<a data-toggle="{{forloop.index}}" href="#">{{item.title}}</a>
		</div>
		<div class="panel-body hidden-element" data-body="{{forloop.index}}"> 
			{{item.body}} 
		</div>
	</div>
	{% endfor %}
{% endraw %}
```

You can see all the CSS and Javascript in the repo here: https://github.com/rdyar/collapsible-panels		

<script>
	const panelHeading = document.querySelectorAll('.panel-heading');
	const panelBody = document.querySelectorAll('.panel-body');
	//loop thru headings to build event listeners
	for (var i = 0, len = panelHeading.length; i < len; i++) {

		panelHeading[i].addEventListener('click', function(e) {
			//set a variable for the heading that matched the one clicked
			const selectedDiv = document.querySelector('[data-body="' + e.target.dataset.toggle + '"]');
			//for each heading also loop thru all bodies
			for (var i = 0, len = panelBody.length; i < len; i++) {
				//if the body does not belong to the heading that was clicked then close it and remove hover style on heading
				if (panelBody[i].dataset.body != e.target.dataset.toggle) {

					panelBody[i].style.height = 0;
					panelHeading[i].className = 'panel-heading';
				}
			}
			//change the height of the body of the clicked heading to collapse or expand it
			if (selectedDiv.clientHeight) {
				selectedDiv.style.height = 0;
			} else {
				selectedDiv.style.height = selectedDiv.scrollHeight + "px";
			}
			//toggle the hover class on the clicked heading
			document.querySelector('[data-toggle="' + e.target.dataset.toggle + '"]').classList.toggle('panel-heading-hover');

		})
	};
</script>
