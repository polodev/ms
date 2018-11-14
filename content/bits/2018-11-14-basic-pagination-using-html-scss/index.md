+++
type="post"
toc=true
title= "Basic Pagination Using Html Scss"
date= 2018-11-14T21:40:51+06:00
draft= false
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
software=['']
bit_tags= ['pagination', 'scss', 'css']
tutorialTypes=['bits']
keyword= "pagination, html pagianation, pure css pagination "
description= "Basic pagination using html and css"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# Html code

~~~html
<div class='pagination'>
      <div class='pagination__inner'>
        <a href="#" disabled><i class="fas fa-chevron-left"></i></a>
        <ul>
          <li class="active"><a href='#'>1</a></li>
          <li><a href='#'>2</a></li>
          <li><a href='#'>3</a></li>
          <li><a href='#'>4</a></li>
          <li><a href='#'>5</a></li>
          <li><a href='#'>6</a></li>
          <li><a href='#'>7</a></li>
          <li><a href='#'>8</a></li>
          <li>...</li>
          <li><a href='#'>41</a></li>
        </ul>
        <a href=#><i class="fas fa-chevron-right"></i></a>
      </div>
    </div>
~~~

# scss code

~~~css
body {
 font-family: verdana, arial, sans-serif;
}
.pagination {
  padding: 3em 0;
  &__inner {
    display: flex;
    justify-content: center;
    align-items: center;
    [disabled]  {
      cursor: none;
      opacity: .5;
    }



     a {
        display: block;
        text-decoration: none;
        padding: 5px 7px;
        transition: all .2s;
        color: #000;
        &:hover {
          color: salmon;
        }
    }
    ul {
      margin: 0 10px;
      padding: 0;
      li {
        display: inline-block;
        &.active, &:hover {
          cursor: pointer;
          a {
            background: rgba(0, 0, 0, 0.1);
            color: salmon;
          }
        }
        a {}
      }
    }
  }
}

~~~

# compiled css code (If your don't prefer scss)

~~~css
body {
  font-family: verdana, arial, sans-serif;
}

.pagination {
  padding: 3em 0;
}
.pagination__inner {
  display: flex;
  justify-content: center;
  align-items: center;
}
.pagination__inner [disabled] {
  cursor: none;
  opacity: .5;
}
.pagination__inner a {
  display: block;
  text-decoration: none;
  padding: 5px 7px;
  transition: all .2s;
  color: #000;
}
.pagination__inner a:hover {
  color: salmon;
}
.pagination__inner ul {
  margin: 0 10px;
  padding: 0;
}
.pagination__inner ul li {
  display: inline-block;
}
.pagination__inner ul li.active, .pagination__inner ul li:hover {
  cursor: pointer;
}
.pagination__inner ul li.active a, .pagination__inner ul li:hover a {
  background: rgba(0, 0, 0, 0.1);
  color: salmon;
}

~~~

# code pen version

<div class='codepen'>
  <p data-height="265" data-theme-id="light" data-slug-hash="XyMxoM" data-default-tab="css,result" data-user="polo-dev-shibu" data-pen-title="Basic Pagination using html scss" class="codepen">See the Pen <a href="https://codepen.io/polo-dev-shibu/pen/XyMxoM/">Basic Pagination using html scss</a> by Polo Dev Shibu (<a href="https://codepen.io/polo-dev-shibu">@polo-dev-shibu</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
</div>

