## Welcome to JavaW's Pages

![img_JavaW](img/JavaW.svg)

### How to use?

* Archives : It is a chronological blog that records some of the problems I have encountered and some of my summaries

* Categories : Divide blogs according to technology stack

* Tags : Divide the corresponding problem tags in the blog


### Some corresponding code comments in this area
```js
// eg:the javascript code in this area
(function() {
var demoItems = document.querySelectorAll('.grid-item')
}());


// eg: the html code in this area
<div class="side ">
  <div>
    <i class="fa fa-th-list"></i>
    Categories
  </div>
  <ul class="content-ul" cate>
    {% for category in site.categories %}
    <li>
      <a href="{{ root_url }}/{{ site.category_dir }}#{{ category | first }}" class="categories-list-item" cate="{{ category | first }}">
        <span class="name">
          {{ category | first }}
        </span>
        <span class="badge">{{ category | last | size }}</span>
      </a>
    </li>
    {% endfor %}
  </ul>
</div>
```



### Support or Contact

See my GitHub for more project source code in [JavaW](https://guides.github.com/w-java).

