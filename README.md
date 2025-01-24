# LLM Setup Instructions

The purpose of this class is to teach you how to behave like a *real* programmer.
Real programmers are allowed to use LLMs for everything they do,
and so you are also allowed to use LLMs for everything you do in this course.
This lab will setup your environment for using LLMs effectively.

<img src=img/oprah.png width=300px />

## Instructions

### Step 0: Installation

We will be using the [llm](https://github.com/simonw/llm) developed by [Simon Wilson](https://simonwillison.net/about/).
This is a popular [CLI tool](https://en.wikipedia.org/wiki/Command-line_interface) for working with llms.
The tool is written in python, and so can be installed with the command
```
$ pip3 install llm
```

Unfortunately, when you run this command, you are likely to get a long error message that looks like
```
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.

    If you wish to install a non-Debian-packaged Python package,
    create a virtual environment using python3 -m venv path/to/venv.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
    sure you have python3-full installed.

    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.

    See /usr/share/doc/python3.12/README.venv for more information.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```

Unfortunately, installing programs in python in [notoriously difficult](https://nielscautaerts.xyz/python-dependency-management-is-a-dumpster-fire.html).

<img src=img/money.jpg width=300px />

Many *package managers* have been introduced to "simplify" this problem.
They include: pip, venv, pip-tools, pipenv, poetry, pdm, pyenv, pipx, uv, conda, mamba, conda-lock, and pixi.
Of course, this hasn't led to simplifying the problem at all.

<a href=https://xkcd.com/927/><img src=img/standards_2x.png width=600px /></a>

The latest version of python has introduced the [*Python Enhancement Proposal* (PEP) 668](https://peps.python.org/pep-0668/),
which forbids mixing of different types of package managers.
The lambda server is an Ubuntu linux system, and so uses the `apt-get` package manager for its system-wide packages.
In order to install packages locally, you will have to use the `venv` package manager.
This is the standard python package manager that you should generally use for python-only projects.

To create a new virtual environment (venv), run the command:
```
$ cd                        # ensure you're in home folder
$ python3 -m venv venv      # create the venv
```
The `-m venv` flag tells `python3` to run the `venv` module.
The last `venv` is the name of the folder that will be created to hold the installed packages.

After running this command, you should notice that a new folder called `venv` has been created.
Run `ls` to verify.

You can activate your venv with the command
```
$ source venv/bin/activate
```
This is called *sourcing* the `venv/bin/activate` file.
Sourcing a file runs the commands in the bash session as if they were copy/pasted into your running terminal.

> **NOTE:**
> The following command is equivalent and also commonly used:
> ```
> $ . venv/bin/activate
> ```
> Because sourcing a file is a common task, the `source` command has been given the synonym `.`.

If everything works correctly, then your prompt will change to look like `(venv) $`.
It is common not to write the `(venv)` portion in tutorials for space reasons.

Now you can install the llm package with the command
```
$ pip3 install llm
```

### Step 1: Installing the Models

The llm package supports many different models and hosting providers.
In this section, we will walk through the steps of setting up <https://groq.com/>.
Groq is free to use, about 20x faster than OpenAI, and supports Meta's latest llama models.

> **NOTE:**
>
> Every model has strengths and weaknesses,
> and there are many benchmarks comparing their performance on different tasks.
> For example: <https://livebench.ai/>.
> The largest llama models are competitive with OpenAI on most tasks, and superior on image tasks.
> Claude tends to be the best on coding tasks.
> Personally, I use groq (with llama) as my default llm due to its speed,
> and then if I need better coding responses, I'll switch to Claude.
> For the purposes of this class, you will do fine if you stay with the free groq provider,
> but you are welcome to use whatever models you would like.
> If you find that some models are better than others for certain tasks in this class, I would love to hear your feedback!

> **NOTE:**
> 
> These instructions are for <https://groq.com>.
> This is a hardware company that has developed new (non-GPU/non-NVIDIA based) silicon for very fast LLM inference.
> Groq is different from [Grok](https://x.ai/),
> which is Elon Musk's LLM company.
> Grok is a new model and designed to compete directly with OpenAI/Meta/Anthropic/Google.
> Groq is a hardware company, and so they are competing with NVIDIA.
>
> Groq has existed as a company for >10 years.
> Grok has existed for ~1 year.
> Musk might be a great businessman,
> but he sucks at naming things.

Install the groq plugin.
```
$ llm install llm-groq
```

Now set the default model to a recent version of llama.
```
$ llm models default groq-llama3.1-70b
```

Next, you need to [create an API key with groq.com](https://console.groq.com/keys).
Once you've done that, register the API key with llm.
```
$ llm keys set groq
```

If everything works, you can now directly ask your llm questions on the command line with a command like
```
$ llm 'why is bash useful for big data?'
```

### Step 2: Example use Cases

The real power of `llm` comes from combining the tool with the POSIX shell.

**Example 1:**
We can use the pipe operator `|` to use `llm` to explain the output of a previous command.
The command below pipes the contents of the `venv/bin/activate` file into `llm`, and asks for an explanation of what the code does.
```
$ cat venv/bin/activate | llm -s "Explain this code"
```

**Example 2:**
We can use output redirection `>` to store the results of the `llm` command in a file.
The command below creates a python file `primes.py` that computes the first 10 prime numbers.
```
$ llm 'write python code that prints the first 10 prime numbers; do not provide any explanation, only valid python' > primes.py
$ python3 primes.py
```

> **Note:**
> LLMs are *non-deterministic*, which means that you can get different results everytime you run the program.
> Sometimes, you might get invalid python code that contains special markdown formating like
> ```
>  ```python
>  print([2, 3, 5, 7, 11, 13, 17, 19, 23, 29])
>  ```
> ```
> By tuning the prompt, you can make this undesired behavior less likely.
> There are some examples of better (but longer) prompts below.

The prompt above is a bit long.
If writing valid python is something that you want to do regularly, you can create an *alias* in the shell.
An alias is like a shortcut for a command.
You can create one like so
```
$ alias llm-python='llm -s "do not provide any explanation or markdown formatting like \`\`\`python, only valid python code that can be run directly in python3"'
```
Now you can create python code by running
```
$ llm-python "print the first 10 prime numbers"
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

primes = []
n = 2
while len(primes) < 10:
    if is_prime(n):
        primes.append(n)
    n += 1
print(primes)
```

```
$ llm-python "write a function that checks if a string is a palindrome; include doctests"
def is_palindrome(s):
    """
    Checks if a string is a palindrome.

    >>> is_palindrome("radar")
    True
    >>> is_palindrome("python")
    False
    >>> is_palindrome("")
    True
    >>> is_palindrome("aibohphobia")
    True
    >>> is_palindrome("hello")
    False
    """
    return s == s[::-1]

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

**Example 3:**
You can embed the output of bash commands into your prompts using the standard command expansion notation `$( )`.
The `uname -a` command prints extensive information about the currently running operating system.
The command below uses `llm` to explain this information
```
$ llm "tell me about my system: $(uname -a)"
```

**More examples:**
You are encouraged (but not required) to [read the documentation](https://llm.datasette.io/en/stable/usage.html),
which contains many more examples.
We will also be seeing more examples throughout the course.

### Step 3 (optional): Adding more LLMs

I have my `llm` configured to use groq by default.
I have also registered Claude, OpenAI, and Gemini.
Anthropic's Claude model has a great reputation as the best coding assistant,
and I occasionally use it to help with coding tasks.
I mostly use OpenAI/Gemini for testing purposes.

You are encouraged (but not required) to get API keys for these other models and try them out.

> **WARNING:**
> It is very easy to use the `llm` command to output many tokens,
> which can cost a lot of money.

I use the following two aliases in my `.bashrc` file to make it easy for me to access the groq and claude models.
```
alias groq='llm -s "keep your response short, between 5-20 lines" -m groq-llama3.1-70b'
alias claude='llm -s "keep your response short, between 5-20 lines" -m claude-3-5-sonnet-latest'
```

## Parting Thoughts

[Laziness is one of the three virtues of a programmer](https://thethreevirtues.com/).
Programmers therefore love tools that automate their work.
So go forth, and let the LLMs automate as much stupid busy work as possible!

<img src=img/replace.jpg width=400px />

<!--
Unfortunately, not all work can be automated.
If you don't use AI correctly,
it will actually INCREASE the amount of work you have to do.

<img src=img/openai.jpg width=400px />

Submit the following statement to sakai:

> I promise to be lazy.
> I promise to use `llm` to avoid as much stupid busy work as possible.
-->
