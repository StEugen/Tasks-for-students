# Some tasks for developing small TO-DO webapp

## Stack
- Flask
- HTML/CSS/Jinja
- PostgreSQL

## Preconfiguration before start
- First you need to install Flask <code>$pip install Flask</code>
- Setup PostgreSQL server



<pre>
Using the if __name__ == '__main__': statement is a best practice in Python when writing scripts that can be used as standalone applications or imported as modules into other programs.

When you execute a Python script directly, such as by running python app.py in the terminal, the __name__ variable is set to '__main__'. On the other hand, if you import the script into another program using the import statement, the __name__ variable will be set to the name of the module.

Using the if __name__ == '__main__': statement ensures that the code inside the block is only executed if the script is being run as the main program. This can be useful in situations where you want to perform certain actions only when the script is being run directly, such as starting a web server or running a standalone command-line application.

Additionally, using this statement can also help avoid unintended side effects when the script is imported as a module into another program, since the code inside the block will not be executed in that case.
</pre>