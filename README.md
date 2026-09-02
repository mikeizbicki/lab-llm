# LLM Setup Instructions

The purpose of this class is to make you a highly productive programmer ready for industry.
Industry programmers are allowed to use LLMs for everything they do,
and so you are also allowed to use LLMs for everything you do in this course.

<img src=img/oprah.png width=300px />

We will be setting up the [llm](https://github.com/simonw/llm) tool developed by [Simon Willison](https://simonwillison.net/about/).
This is a popular [CLI tool](https://en.wikipedia.org/wiki/Command-line_interface) for working with llms.
It supports the latest models (like Fable 5.1, which was released 1 September 2026) and all model providers (e.g. OpenAI, Anthropic, Google, Deepseek, Qwen) with a consistent interface.

> **RECALL:**
> The technology policy for this class forbids you from using web-based tools like <https://chatgpt.com>.
> That is because these "user friendly" AI tools are less powerful than the tools working programmers use.
> In this class, you are only allowed to use the more powerful tools that we cover in class.
> This will encourage you to develop good habits that will make you more productive in the long run.

## Instructions

### Step 0: Installation

`llm` is written in python, and so can be installed with the command
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

The latest version of python has introduced [*Python Enhancement Proposal* (PEP) 668](https://peps.python.org/pep-0668/),
which forbids mixing of different types of package managers.
The lambda server is an Ubuntu linux system, and so uses the `apt-get` package manager for its system-wide packages.
In order to install packages locally, you will have to use the `venv` package manager.
`venv` was introduced in 2011 with [PEP 405](https://peps.python.org/pep-0405/),
and is the standard python package manager for python-only projects.

To create a new virtual environment (venv), run the command:
```
$ cd                        # ensure you're in home folder
$ python3 -m venv venv      # create the venv
```
The `-m venv` parameter tells `python3` to run the `venv` module.
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
>
> > **DOUBLE NOTE:**
> > Observe the punctuation of the periods above---there are two dots, one with a grayed background and one with a normal background.
> > At this point, you should understand both the *syntax* of how to create this effect (using single backticks in markdown to denote inline code) and the *semantics* of what tthis effect means (the dot inside of the backticks is literal code that can be executed).
> > Programmers constantly use weird symbols when coding, and correct use of markdown is needed to ensure that others can properly read and interpret what we write.

If everything works correctly, then your prompt will change to look like `(venv) $`.

> **NOTE:**
> Prompts can vary widely based on user preferences and the history of commands they have run.
> It is common not to write `(venv)` or anything else before the `$` due to this variability and to save space.

Now you can install the llm package with the command
```
$ pip3 install llm
```

### Step 1: Installing the Models

The llm package supports many different models and hosting providers.
In this section, we will walk through the steps of setting up <https://groq.com/>.

> **NOTE:**
> Groq (with a q) is a hardware company that has developed new silicon for very fast LLM inference.
> [Grok](https://x.ai/) (with a k) is Elon Musk's LLM company.
> Groq is a hardware company, and so they are competing with NVIDIA.
> Grok is a software company competing with OpenAI/Meta/Anthropic/Google.

First, you need to [create an API key with groq.com](https://console.groq.com/keys).
Groq has a nice free tier, so there is no need to pay.
Then register the API key with llm.
```
$ llm keys set groq
```

Groq supports many different models, but we will use Qwen3.8.
This model was released as open weight on 16 August by the Chinese company Alibaba,
and is competitive with SOTA models from OpenAI/Anthropic at coding tasks.

<img src=img/qwen.png width=400px />

To register the model with llm, edit the file `~/.config/io.datasette.llm/extra-openai-models.yaml` by running the command:
```
$ cat >> ~/.config/io.datasette.llm/extra-openai-models.yaml <<'EOF'
- model_id: qwen
  model_name: qwen/qwen3.8-27b
  api_key_name: groq
  api_base: https://api.groq.com/openai/v1
EOF
```
> **NOTE:**
> Observe that the output redirection above will append the contents of the heredoc to the end of the config.
> Commands like this are common in tutorials.
>
> If you we were not using the shell,
> modifying these condif files would require more complicated instructions that are not easy to automate---something like "add the following text to the end of the file"---and you would have had to click a bunch of buttons in VSCode to make that happen.
> With the shell, I give you a one line command to copy/paste that does absolutely everything.
>
> The shell may be uncomfortable at first,
> but once you are used to the shell it becomes much faster and easier because of the ability to automate.
> LLMs are especially good at giving you shell commands that you can just copy/paste to get your desired effects.

<!--
```
$ llm openai endpoint https://api.groq.com/openai/v1 -m qwen/qwen3.8-27b --key groq
```
The key parts of the command are
1. `openai endpoint https://api.groq.com/openai/v1 --key groq` tells `llm` to use the groq api endpoint with the key you set above.
2. `-m qwen/qwen3.8-27b` sets the model that the groq endpoint will serve.
    This model was released in 16 August and is quite good at coding tasks.
    You can find a full list of supported models at <https://console.groq.com/docs/models>.
3. `-s 'answer like a pirate in 2 pages'` sets the *system prompt* for the llm query.
    The system prompt is used to adjust the style of the response.
4. The last part `'why is bash important for data analysts?'` is the prompt for the model, which is what the model will actually respond to.
At this point, you should be able to run the command above and get an LLM-generated output about why bash is important.

The command above, however, is obviously unwieldy.
We can use *bash aliases* to make the command more ergonomic.
An alias is just a short name for a longer command.
Create the alias `qwen` with the command
```
$ alias qwen="llm openai endpoint https://api.groq.com/openai/v1 -m qwen/qwen3.8-27b --key groq -s 'respond in 1-20 sentences as efficiently as possible'"
```
(Notice that I changed the system prompt between the first command and the alias above.)
-->

Now, we can ask qwen questions with a command like
```
$ llm -m qwen 'why is bash important for data analysts?'
```
Notice that the results being returned are much faster than the results if you were to use the <https://chatgpt.com> interface because groq has such fast inference hardware.

Also notice that the response is long!
Much longer than I want to read!

You can control the length and style of the response by setting the *system prompt* with the `-s` argument:
```
$ llm -m qwen -s 'answer in 1-2 sentences' 'why is bash important for data analysts?'
```
Explicitly setting the model and system prompt everytime we call the `llm` command is a bit awkward.
In bash, we can create shortcuts for long commands using the `alias` keyword.
Run the command
```
$ alias qwen="llm -m qwen -s 'answer in 1-2 sentences'"
```
Now we can ask the qwen model questions like
```
$ qwen 'why is bash important for data analysts?'
```
At this point we have a convenient command line tool for working with LLMs.
Our tool is already more powerful than the web interfaces because:
1. it is faster, and
2. we can control the formatting/style by adjusting the system prompt.
In the Section 2 below, we will see how to combine our `qwen` command with standard bash features to get even more power.
But first, we need to discuss how to "save" these changes to our system.

#### Step 1a: .bashrc

One of the important skills to learn when using the shell is what commands are *persistent* (their effects will last after you logout and log back in) and what commands are *ephemeral* (their effects will go away after you log out).
One of the key principles of shell programming is that **modifying files is persistent, everything else is ephemeral**.
Based on this principle, we can determine which of our commands have been persistent and which ephemeral.

First, log out of the lambda server by running
```
$ exit
```

<img src=img/pooh.jpg width=300px />

Then log back into the lambda server.

Recall that we registered the `qwen` model with `llm` by using the bash append operator `>>` to modify the file `~/.config/io.datasette.llm/extra-openai-models.yaml`.
Because this command effected a file, the change was persistent, and we still have access to qwen.
So the following command should work:
```
$ llm -m qwen 'what is qwen?'
```

But we also said commands like this were awkward to run, especially with a custom system prompt, and so we created a bash alias called `qwen`.
The `alias` command in bash does not modify any files, and so is not persistent.
The following command should fail.
```
$ qwen 'what is bashrc?'
bash: qwen: command not found
```
To make these commands persistent, we need to write them to a file.
A common way to do this is to add these ephemeral commands to your `~/.bashrc` file,
and then bash will run these commands everytime you login automatically.

> **NOTE:**
> Recall that `~` is your home folder.
> So `~/.bashrc` is a file named `.bashrc` located in your home folder.
> If you run
> ```
> $ cd ~
> $ ls
> ```
> you will not see a file called `.bashrc`.
> `.bashrc` is a *hidden file* because it starts with a `.` and is not displayed by default.
> To display hidden files, add the `-a` flag to the `ls` command:
> ```
> $ ls -a
> ```
> You should see a number of hidden files displayed, including the `.bashrc` file.

> **TASK YOU MUST COMPLETE:**
>
> Modify the `.bashrc` file to include the alias for the `qwen` command:
> ```
> alias qwen="llm -m qwen -s 'answer in 1-2 sentences'"
> ```
> Notice that there is no `$` at the beginning of the line above;
> that is because this is not a terminal command that you should be typing into the shell, but the literal characters that you will be adding to a file that just happens to also be a valid shell command.
> Subtle grammar points like this are important!
> As a reader, they give you clues that help you check your understanding of technical content; as a writer, they signal competence to your readers and will make people want to hire you.
>
> You may use vim or `>>` to add the alias, whatever is easier.

### Step 2: Example use Cases

The real power of `llm` comes from combining the tool with the POSIX shell.
In this section, we will see some basic examples.

**Example 1:**

We can use the pipe operator `|` to use `llm` to explain the output of a previous command.
For example, the command `uname -a` shows a compressed version of the system information.
You can get a more beginner friendly explanation by piping this output into qwen with a modified system prompt:
```
$ uname -a | qwen -s 'explain the output of the command'
```

**Example 2:**

One downside of using the `-s` operator is that it overwrites our default system prompt, and we got very verbose output.
Another way to create prompts is via heredocs and command substitution.
The command below is similar to the command above but reuses are standard system prompt.
```
$ qwen <<EOF
Explain the output of the command:

$(uname -a)
EOF
```

** Example 3:**

It is common to want to ask follow up questions to llms.
For example, maybe we want to know if our system has any security vulnerabilities?
(Recall that you can get extra credit by hacking the lambda server---it does in fact have some unpatched vulnerabilities.)
We can use the `-c` flag to continue our conversation from the previous command to ask these followup questions.
```
$ qwen -c 'are there any known security vulnerabilities?'
```
Asking thoughtful followup questions will lead the motivated student to find a way to hack the lambda server and claim extra credit.

** Example 4:**

Another common usecase for `llm` is to ask questions about a file.
For example, we've previously "sourced" the `venv/bin/activate` file but we haven't talked at all about how it works.
If you're curious, you can ask qwen with a command like:
```
$ qwen <<EOF
How does this file work?

$(cat venv/bin/activate)
EOF
```
And we can ask followup questions using the `-c` flag again like
```
$ qwen -c 'does the file have any bugs that need fixing?'
```
Web interfaces have much more friction to getting files into the LLM "context window",
and are consequently much harder to use.

**Example 5:**

Another common use case for llms is to write code for us.
For example:
```
$ qwen 'write a python program that prints the first 10 prime numbers'
```
We can easily use output redirection `>` to store the results of the `llm` command in a file.
Notice that when printing code, qwen (like most LLMs) typically puts the code inside of a markdown code block.
The output I got from running the command above looked like

    ```python
    def is_prime(n):
        if n < 2:
            return False
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                return False
        return True

    primes = []
    num = 2
    while len(primes) < 10:
        if is_prime(num):
            primes.append(num)
        num += 1

    print(primes)
    ```

> **NOTE:**
> Your output will be slightly different because LLMs are *non-deterministic*.

> **NOTE:**
> LLMs use markdown formatting because they are trained on the internet,
> and (good!) programmers always embed their code inside of markdown codeblocks like this when writing on the internet.

When asking an LLM to generate code, it is often the case that we want to remove this extra markdown formatting in order to generate valid python code.
The `-x` flag removes the markdown formatting for us:
```
$ qwen -x 'write a python program that prints the first 10 prime numbers'
```
Now, we can use standard output redirection to save these files for us automatically.
```
$ qwen > primes.py -x 'write a python program that prints the first 10 prime numbers'
$ python3 primes.py
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

**More examples:**

You are encouraged (but not required) to [read the documentation](https://llm.datasette.io/en/stable/usage.html),
which contains many more examples.

We will also be seeing more examples throughout the course.

### Step 3: Adding more LLMs

`llm` can be used with any LLM provider (e.g. OpenAI, Anthropic, Gemini, Deepseek, Qwen).
You are encouraged (but not required) to add more LLM providers and models to your `llm` command.
I like the service <https://openrouter.ai/> because it allows easily using any model with only one payment endpoint.
There is a plugin `llm-openrouter` that automates the addition of hundreds of these models to your system,
and you can find setup instructions at <https://github.com/simonw/llm-openrouter>.

> **NOTE:**
> There are no restrictions on what models/APIs you are allowed to use in this class.
> The only restrictions will be on *how* you access them.
> Anything you can do with the `llm` command is 100% allowed.

> **WARNING:**
> At various times in this class you will have to put the `llm` command inside of for loops.
> It is very easy to generate large bills using these paid APIs.

## Submission

Your submission will have you practice both github and markdown.

You must:

1. Fork this repo.

1. Modify the code block below so that it contains the output of the command below
    ```
    $ qwen 'what is .bashrc?'
    <add_your_programs_response_here>
    ```

1. Push your changes to github.

1. Submit a link of the modified repo to canvas.

    You will only get credit if all of these steps have been completed correctly.

> **NOTE:**
> It is okay if the git steps above take you a long time to figure out at this point in the class.
> But by the end of the class, you will be able to do all of these steps in <30 seconds.
> These are very natural tasks that working programmers perform many times a day.

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
-->
