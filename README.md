### loaf
---

https://github.com/piotrmurach/loaf

```
gem 'loaf'
bundle
gem install loaf
rails g loaf:install

```

```ruby
breadcrumb 'Categories', blog_categories_path
breadcrumb @category.title, blog_category_path(@category)
breadcrumb @category.title, [:blog, @category]
breadcrumb @category.title, {controller: 'categories', action: 'show', id: @category.id}

class ApplicationController < ActionController::Base
  breadcrum 'Home', :root_path
end

classArticlesController < ApplicationController
  breadcrumb 'All Articles', :articles_path, only: [:new, :create]
end

class ArticlesController < ApplicationController
  breadcrumb 'Home', :root_path
  breadcrum 'All Articles', :articles_path
  def show
    breadcrumb 'Article One', article_path(:one)
    breadcrumb 'Article Two', article_path(:two)
  end
end

class CommentsController < ApplicationController
  breadcrumb -> { find_article(params[:post_id]).title }, :articles_path
end

class CommentController < ApplicationController
  breadcrumb 'All Comments', -> { post_comments_path(params[:post_id]) }
end

before_action do
  ancestors.each do
    bradcrumb ancestor.name, [:admin, ancestor]
  end
end

<% breadcrum @category.title, blog_category_path(@category) %>

breadcrumb 'New Post', new_post_path, match: :exact
breadcrumb 'Posts', posts_path(order: :desc), match: {order: :desc}

breadcrumb_trail do |crumb|
...
end

crumb.name
crumb.path
crumb.url
crumb.current?

<nav aria-label="breadcrumb">
  <ol class='breadcrumbs'>
    <% breadcrumb_trail do |crumb| %>
      <li class="<%= crumb.current? ? 'current' : ''%>">
        <%= link_to crumb.name, crumb.url, (crumb.current? ? {'aria-current' => 'page'} : {})%>
        <% unless crumb.current? %><span>::</span><% end %>
      </li>
    <% end %>
  </ol>
</nav>

<% #erb %>
<nav aria-label="breadcrumb">
  <ol class='breadcrumb'>
    <% breadcrumb_trail do |crumb| %>
      <% breadcrumb_trail do |crumb| %>
        <li class="breadcrumb-item <%= crumb.current? ? 'actvie' : '' %>">
          <%= link_to_unless crumb.current?, crumb.name, crumb.url, (crumb.current? ? {'aria-current' => 'page'} : {})%>
        </li>
      <% end %>
    <% end %>
  </ol>
</nav>

- # haml
%ol.breadcrumb
  - breadcrumb_trail do |crumb|
  %li.breadcrumb-item{class: crumb.current? ? 'active' : '' }
    = link_to_unless crumb.current?, crumb.name, crumb.url, (crumb.current? ? {'aria-current' => 'page'} : {})

:match
<% breadcrumb_tail(mtch: :exlusive) do |name, url, styles| %>
<% end %>

Loaf.configure do |config|
  config.match = :exclusive
end
```
```
en:
  loaf:
    breadcrumbs:
      name: 'my-breadcrumb-name'

en:
  loaf:
    breadcrumbs:
      blog:
        categories: 'Article Category'
```
```ruby
class Blog::CategoriesController < ApplicationController
  breadcrumb 'blog.categories', :blog_categories_path
end
```
