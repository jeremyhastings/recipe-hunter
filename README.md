# Recipe Hunter

## Background

[Recipe Puppy website](http://www.recipepuppy.com/), which is an ingredient based recipe search engine, offer an API that returns recipes in JSON format.

1. Fetch the recipe information based on a search term entered in the address bar
2. Display the information in an HTML table - specifically, the **recipe photo**, the **recipe title** (both of which would be links that can take you to the actual recipe) and the **ingredients** required for the recipe.

## Getting Started

#### Prerequisites

This template was tested with Ruby **2.6.3** and Ruby on Rails **4.2.11**.

#### Bootstrapping

1. Clone this repo
2. Run `bundle install` to install all the necessary gems

## Requirements

1. Generate a `recipes` controller that has an `index` action

```shell
rails generate controller recipes index
```

2. Inside the `index` action, create 2 instance variables:
   a. **`@search`**- this is the value of the query parameter `search` if one is provided or `chocolate` if no `search` query parameter is present (default).
   b. **`@recipes`** - this is an array of recipes that you will get by calling the class method `for` of the `Recipe` class (_model_) available under `app/models/recipe.rb` with the `@search` variable created above.

```shell
  def index
     	@search = params[:search] || 'chocolate'
  	   @recipes = Recipe.for(@search)
  end
```

3. The view, `app/views/recipes/index.html.erb`, should contain the following:
   a. An `<h1>` element with "Recipes for X" (where 'X' would be replaced with the actual search term used). Look back at your `controller` implementation for what you can use to insert a search term here.
   b. A `<table>` element

```shell
<h1>Recipes for <%= @search %></h1>
<table></table>
```

6. The `<table>` element from the previous list item should contain the following:
   a. `<tr>` element with 3 `<th>` elements for `Photo`, `Title` and `Ingredients` respectively.
   b. `<tr>` element for each `recipe` in the `recipes` array with 3 `<td>` elements (columns) as described below.
   
   1st column should contain an image of the recipe (`thumbnail` property of the `recipe`) hyperlinked to the recipe's URL (`href` property of the `recipe`). You can use [link_to](https://api.rubyonrails.org/v4.2.11/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to) and [image_tag](https://api.rubyonrails.org/v4.2.11/classes/ActionView/Helpers/AssetTagHelper.html#method-i-image_tag) view helpers to accomplish this task.
   2nd column should contain the title of the recipe (`title` property of the `recipe`). Again, you can use `link_to` view helper to accomplish this task.
   3rd column should contain the ingredients required for the recipe (`ingredients` property of the `recipe`).

```shell
<table>
	<tr>
		<th>Photo</th>
		<th>Title</th>
		<th>Ingredients</th>
	</tr>
	<% @recipes.each do |recipe| %>
		<tr>
			<td><%= link_to image_tag(recipe["thumbnail"]), recipe["thumbnail"] %></td>
			<td><%= link_to recipe["title"], recipe["href"] %></td>
			<td><%= recipe["ingredients"] %></td>
		</tr>
	<% end %>
</table>
```

7. Right now, the only way to get to the `index` action of the `recipes` controller is to actually type in `recipes/index` in the browser URL. It would be nice if the **root** route of the application automatically took us to the `index` action of the `recipes` controller. For this to happen, you will need to do something in the `config/routes.rb`

```shell
root 'recipes#index'
```

8. Some characters come back from the API as [HTML entities](https://developer.mozilla.org/en-US/docs/Glossary/Entity), but should be displayed correctly on the page. Let's assume that we only want to fix this issue in the **title** of the listing. 

```shell
      <td><%= link_to sanitize(recipe["title"]), recipe["href"] %></td>
```

## Running the tests

1. Install the following gems. You may already have them installed.

    ```shell
    $ gem install rspec
    $ gem install rspec-its
    ```

3. Run the rspec command from the project root directory to execute the unit tests within the spec directory.
    
    ```shell
    $ rspec
    ```

## Built With

* [Ruby](https://www.ruby-lang.org/en/) - Language
* [Ruby on Rails](https://rubyonrails.org) - MVC Framework
* [RubyMine](https://www.jetbrains.com/ruby/) - IDE

## Contributing

If you want to ...

## Authors

* **Jeremy Hastings** - *Initial work* - [Jeremy Hastings](https://github.com/jeremyhastings/)

## License

This project is licensed under the GNU General Public License 3.0 License - see the [LICENSE.md](LICENSE.md) file for details
