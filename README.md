# TvmazeApiGem

This is how you start a gem from scratch for your CLI project:

### Creating your GitHub Repository
https://github.com/new is where you make a new repo
make sure you don’t add a README or License or anything else, just give it a name (and description if you want). The reason for this is so that your new github repo doesn't have any commits.

-When you get to your repo's page on github there will be 2 code blocks to help you get started, the top one has more lines in it, the bottom one just has 2, you want to copy those lines for use in a later step. They'll look something like this:
```
git remote add origin git@github.com/your_github_username/name_of_your_repo.git
git push -u origin master
```

### Creating your CLI Gem
Next, you'll want to create the gem on your machine using bundler. Before we do that, though, let's figure out what we want to call our gem. Check https://rubygems.org to see if your gem name is already taken, and try to find one that's unique. Generally, gems will be named in snake case (lower case with underscores between words). The name is important as it will also match the name of the module that bundler creates for you and the files and folders inside of your lib directory that will be created when you build the gem.

After you've settled on a properly formatted name that is unique and you like, you'll want to navigate to the folder that you want to have your project inside of. If you're okay having the project in your home directory, you can just type `cd` into your terminal and hit enter before you do this next step. 

### Using Bundler to build a Gem
To create your gem you do this in your terminal:
```
bundle gem my_awesome_gem_name_that_i_just_chose_in_the_previous_step
```
-then to open it up in VS Code:
`code my_awesome_gem_name_that_i_just_chose_in_the_previous_step` 
### Making our First Commit to our Local Repository
In the terminal within VSCode:
```
git add .
```
then 
```
git commit -m "initial commit"
```
and then paste in the 2 lines from github and hit enter to assign our github repo as the remote for our local repo for the gem.

when you refresh the page on GitHub you should see your code there

## Updating the gemspec file

### spec.summary
Add a summary of your gem that will appear on Rubygems.org if you choose to deploy it. Put this between the %q{} like this:
```
spec.summary       = %q{TVMaze CLI Gem}
```
### spec.description
Add a longer description of your gem that will also appear on RubyGems:
```
spec.description   = %q{Provides a CLI you can run to see info about shows from the TVMaze API.}
```
### spec.homepage
you can add your github repo as the homepage for now:
```
spec.homepage      = "https://github.com/DakotaLMartinez/tvmaze_api_gem"
```
### spec.metadta["allowed_push_host"]
If you want to publish your gem on RubyGems.org eventually. You'll want to remove or comment out this line:
```
# spec.metadata["allowed_push_host"] = "TODO: Set to 'http://mygemserver.com'"

```
### spec.metadata["source_code_uri"]
make this equal to your github repo as well:
```
  spec.metadata["source_code_uri"] = spec.homepage

```

### spec.metadata["changelog"]
you can remove this for now.
```
# spec.metadata["changelog_uri"] = "TODO: Put your gem's CHANGELOG.md URL here."
```

### spec.bindir
We'll be using the bin directory for our executable files so we don't need to specify another directory here. Let's remove this line:
```
# spec.bindir        = "exe"
```

### spec.executables 
Here we want to specify the name of our executable file that will start our CLI. This will ensure that bundler adds the executable to our user's path so they can run our cli by typing the name of our bin file.
```
spec.executables   = ["tvmaze_cli"]
```
(this should be the name of your executable file not mine)

### spec.add_dependency
If your CLI data gem relies on other gems to deliver its functionality, you'll want to make sure that bundler knows what gems your gem depends on. You can do that by adding dependencies to the bottom of your gemspec like so:
```
spec.add_dependency "http"
```

## Loading Files and Other Plumbing Issues
Your gem's executable file, should look something like this:
```
#!/usr/bin/env ruby

require "bundler/setup"
require_relative "../lib/tvmaze_api_gem"

TvmazeApiGem::CLI.new.start
```
The top line here is called a shebang. It tells bash to interpret (run) this file using the ruby interpreter. This will allow you to run the file by calling its name without first adding ruby. (you can do `tvmaze_cli` instead of `ruby tvmaze_cli` in your terminal). 

`require` and `require_relative` are both used to load other ruby files. When you use `require` ruby will look through all of the directories in the `$LOAD_PATH` to find a file that matches the path you pass in. 

When you use `require_relative` ruby will look for a file at the path relative to the path to the file in which the `require_relative` occurs. For example, if you run `require_relative` from the bin directory and want to load a file inside of the lib directory, you would prefix your path with a `..`. This `..` refers to the parent of the directory in which the file appears, in this case that's the root of the project, where both the `bin` and `lib` directories are.

As a general rule, you can use `require_relative` to load your own code and it should work no matter where you run the code from. You can also load your own code using `require`, but things can get a bit more complicated because of the `$LOAD_PATH`. If you're loading files defined in another gem, you're most definitely going to use `require`

### One file to rule them all
```
require_relative "../lib/tvmaze_api_gem"
```
In your case, `tvmaze_api_gem` will be the name of the gem that you chose. This file will require all of the other files and dependencies needed to run your program. It will look something like this:

```
require "http"
require_relative "tvmaze_api_gem/version"
require_relative "tvmaze_api_gem/api"
require_relative "tvmaze_api_gem/cli"
require_relative "tvmaze_api_gem/tv_show"

module TvmazeApiGem
  class Error < StandardError; end
  # Your code goes here...
end

```

When you create your files within the `lib/my_awesome_gem_name_that_i_just_chose_in_the_previous_step` folder, their names should match the `require_relative` statements inside of the `lib/my_awesome_gem_name_that_i_just_chose_in_the_previous_step.rb` file.  When you define the classes inside of those files, you generally want to match the file names with the class names, though at this point there isn't actually a rule that you have to do this, later it will matter.

For example:
```
#lib/tvmaze_api_gem/api.rb
module TvmazeApiGem
  class API 

  end
end
```
```
#lib/tvmaze_api_gem/cli.rb
module TvmazeApiGem
  class CLI 
    def start 
      puts "Hello! It's working!!! :D"
    end
  end
end
```
```
#lib/tvmaze_api_gem/tv_show.rb
module TvmazeApiGem
  class TVShow 

  end
end
```

## Updating your Repo on GitHub

Once you've got some code working and you want to share it with the world, you can do so by running the following git commands:

``` 
git add .
```
This command will add all of the changes in your working directory to a staging area so they can be commited to your local repository.
```
git commit -m "a message describing what I did"
```
This command actually puts the changes into the repository, creates a SHA reference to the change and attaches your message to it.

```
git push
```
This command takes your local changes and pushes them to the remote version of the branch (in this case your GitHub repository)

You'll also want to take a look at the README.md file that was created for you and add any relevant information there. For example, if your project relies on an API, you can add instructions here on how to access the API documentation and get an API key if necessary. 
