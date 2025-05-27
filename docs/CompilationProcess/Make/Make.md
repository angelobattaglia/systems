# Make

The mechanics of programming usually follow a fairly simple routine of editing source files, compiling the source
into an executable form, and debugging the result. The make program is intended to automate the mundane aspects of
transforming source code into an executable. The advantages of make over scripts is that you can specify the relationships
between the elements of your program to make, and it knows through these relationships and timestamps exactly what
steps need to be redone to produce the desired program each time. Using this information, make can also optimize the
build process avoiding unnecessary steps. 
[O'Reilly Managing Projects with GNU Make, Third Edition By Robert Mecklenburg November 2004](https://www.oreilly.com/openbook/make3/book/)

### Install `cmake` if you need it
If you need `cmake` just install it
```text
Install CMake using pip3 and link it for system-wide access:

pip3 install cmake==3.31.6
sudo ln -sf /usr/local/bin/cmake /usr/bin/cmake
cmake --version
```

### What is `.PHONY` and how to use it

`.PHONY` tells `make`:

> "This target is **not a real file**. It's just a **name for a command** I want to run."

---

### Why use it?

https://www.skenz.it/cs/makefile_1
Target listed after the keyword “.PHONY” are executed regardless the existence of a file with the same name that
has been edited more recently than the dependencies.

Imagine you have this in your Makefile:

```make
clean:
	rm -f *.o prog
```

If a **file named `clean`** exists in your folder, `make` will say:

> "clean already exists — nothing to do."

😬 The command won't run!

### How to fix it:

```make
.PHONY: clean

clean:
	rm -f *.o prog
```

Now, even if a file called `clean` exists, `make clean` will **still run**.

### Summary

* `.PHONY` is for targets that are **not files**.
* It **forces make to always run them**.
* Common examples: `clean`, `all`, `install`, `test`.

**Make doesn’t know whether a target is a command or a file — it assumes every target might be a file.**

### 🧠 Why does this happen?

`make` was designed to **build files**. That’s its original purpose. So by default:

* If you write:

  ```make
  clean:
  	rm -f *.o
  ```

  `make` assumes `clean` might be a file you're trying to create.
* If a file named `clean` exists in your directory, `make` will say:

  > "`clean` is up-to-date — skip the recipe."

So `make` skips the command entirely, thinking the "target" (`clean`) is already "built".

---

### 🛠 Why `.PHONY` exists

`.PHONY` **tells make:**

> "Hey, don’t treat this as a file — this is a fake target. Always run it."

That’s why you do this:

```make
.PHONY: clean
clean:
	rm -f *.o prog
```

Now, `make` won’t care if there’s a file called `clean` — it will always run the `clean:` recipe.

---

### ⚙️ Summary

| Without `.PHONY`                         | With `.PHONY`            |
| ---------------------------------------- | ------------------------ |
| `make` checks if target exists as a file | `make` skips file checks |
| Might skip your recipe                   | Always runs your recipe  |
| Confusing when debugging                 | Predictable and safe     |

---

### 🧩 Big idea:

> `make` is a *file generator by design*. If you're using it for *commands* (like `clean`), you must **tell it explicitly**.

Here's a quick **cheat sheet** of common `.PHONY` targets used in Makefiles:

---

### ✅ Common `.PHONY` Targets

| Target      | Purpose                                                   |
| ----------- | --------------------------------------------------------- |
| `all`       | Build everything (often the default target)               |
| `clean`     | Delete all compiled files and output (e.g. `*.o`, `prog`) |
| `distclean` | Like `clean`, but also remove generated directories/files |
| `install`   | Copy the final program or files into a system location    |
| `uninstall` | Remove files that were installed                          |
| `test`      | Run tests                                                 |
| `check`     | Alternative to `test`; runs validations                   |
| `run`       | Run the program after building                            |
| `rebuild`   | Clean and rebuild everything                              |
| `help`      | Print help or usage info about available targets          |

---

### 🧾 Example

```make
.PHONY: all clean install run

all: myprogram

myprogram: main.o
	g++ -o myprogram main.o

clean:
	rm -f *.o myprogram

install:
	cp myprogram /usr/local/bin

run: myprogram
	./myprogram
```

With `.PHONY`, `make` will run these even if files named `clean`, `install`, or `run` exist.

---

Would you like this as a downloadable PDF cheat sheet too?
