---
---
#### Partials

Partials allow you to extend a view and inject content into it, inline, within your views.


#####  Creating a Partial
Partials start with the `@partial('path.to.view')` directive, which accepts the view you want the partial to extend from, and end with the `@endpartial` directive.

{% highlight html %}
@partial('partials.panel')
    @block('title', 'This is the panel title')

    @block('body')
        This is the panel body.
    @endblock
@endpartial
{% endhighlight %}

Blocks within partials behave the same way as sections within templates. They capture a piece of data that will be rendered into the extended view.

#### Rendering a Partial
We use the `@render('block-to-render')` directive to render a block of content that was provided via the respective `@block` directive. Note that we can also provide a default value.

{% highlight html %}
<div class="panel">
    <div class="panel-heading">
        <h3 class="panel-title">@render('title', 'Default Title')</h3>
    </div>
    <div class="panel-body">
        @render('body', 'Default Body')
    </div>
</div>
{% endhighlight %}

#### Full Example
This example will include our partial HTML file. Notice that we can create as many instances of the partial as we like.

{% highlight html %}
// index.blade.php
@extends('layouts.master')

@section('content')
    @partial('partials.panel')
        @block('title', 'This is the panel title')

        @block('body')
            This is the panel body.
        @endblock
    @endpartial

    @partial('partials.panel')
        @block('title', 'This is a second panel title')

        @block('body')
            And we will have different content in this body.
        @endblock
    @endpartial
@stop

// /partials/panel.blade.php
<div class="panel">
    <div class="panel-heading">
        <h3 class="panel-title">@render('title')</h3>
    </div>
    <div class="panel-body">
        @render('body')
    </div>
</div>
{% endhighlight %}

#### Nesting Partials
You can also do some cool things by nesting partials. For example:

{% highlight html %}
// index.blade.php
@extends('layouts.master')

@section('content')
    @partial('partials.danger-panel')
        @block('title', 'This is the panel title')

        @block('body')
            This is the panel body.
        @endblock
    @endpartial
@stop

// partials/danger-panel.blade.php
@partial('partials.panel')
    @block('type', 'danger')

    @block('title')
    	Danger! @render('title')
    @endblock
@endpartial

// partials/panel.blade.php
<div class="panel panel-@render('type', 'default')">
    <div class="panel-heading">
        <h3 class="panel-title">@render('title')</h3>
    </div>
    <div class="panel-body">
        @render('body')
    </div>
</div>
{% endhighlight %}