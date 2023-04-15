---
title: "Cracking the CKAD Code: Lessons Learned from My Exam Experience"
categories:
- Exam Overview
- DevOps
- Exam Preparation
- Kubernetes
- Certification

exerpt: "If you're looking to deepen your knowledge of Kubernetes and demonstrate your expertise to potential employers, the Certified Kubernetes Application Developer (CKAD) certification is a great way to do so. As someone who recently passed the CKAD exam, I can attest to the value of this certification and the benefits it can bring.

The CKAD certification is designed to test your skills in designing, building, and deploying cloud-native applications using Kubernetes. It's a hands-on exam that requires you to complete a set of tasks within a given time frame, using a real Kubernetes cluster. This means you need to be comfortable with the command line and able to work quickly and efficiently in a Kubernetes environment."
---

**Overview**

The Certified Kubernetes Application Developer (CKAD) certification is a program created by the Cloud Native Computing Foundation (CNCF) that tests developers' skills in designing, building, configuring, and exposing cloud-native applications for Kubernetes. The certification exam is a hands-on, performance-based test that requires you to solve a set of problems using a live Kubernetes cluster. The exam is designed to test your ability to work with Kubernetes in a real-world environment and evaluate your ability to use the Kubernetes API primitives to design, build, and deploy cloud-native applications.

The exam is two hours long and consists of a set of performance-based tasks that you need to complete using a live Kubernetes environment. You'll be provided with a list of tasks that you need to complete in a given amount of time, and you'll need to use Kubernetes command-line tools to solve the problems. The exam tests your ability to deploy, manage, and scale applications using Kubernetes, and it covers a range of topics, including core concepts, networking, scheduling, storage, security, and troubleshooting.

To prepare for the CKAD exam, CNCF recommends that you have a strong understanding of Kubernetes concepts and commands. They also recommend that you complete the Kubernetes Fundamentals course, which is available for free on edX. There are also many other resources available online, including practice exams, study guides, and online courses.

The CKAD certification is a valuable credential for developers who are interested in cloud-native development and want to demonstrate their expertise in Kubernetes. It can help you stand out in a competitive job market and open up new opportunities for career growth.

**killer.sh - https://killer.sh/ckad**

After purchasing the exam for CKAD, you will be given two session of `killer.sh`. Each session lasts 36 hours. Use these to practice on a simulator environment that mimics as much as possible the real exam.

The simulator is more difficult than the real exam, so it provides a higher learning curve.

**Terminal Setup**

When it comes to the CKAD exam, time management is key. The exam is designed to be challenging and fast-paced, and you'll need to work quickly and efficiently to complete all the tasks within the allotted time. That's why it's important to have a plan in place before you start the exam, and to use your time wisely.

One way to do this is to take advantage of the first few minutes of the exam to set up your working environment. This can include customizing your terminal settings, setting up aliases for commonly used commands, and configuring any other tools or resources that you'll need during the exam.

**Bash Aliases**

Here's the contents of my `v~/.bash_aliases` file:

```bash
alias k=kubectl
alias kn="kubectl config set-context --current --namespace"
alias ka="kubectl apply -f"

export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
```

- `alias k=kubectl`: This sets up an alias for the 'kubectl' command, allowing you to type 'k' instead of 'kubectl'. This can save time and make your commands easier to type.

- `alias kn="kubectl config set-context --current --namespace"`: This sets up an alias for the `kubectl config set-context` command with the `--current` and `--namespace` options. This allows you to switch between namespaces quickly and easily without having to remember the full command.

- `alias ka="kubectl apply -f"`: This sets up an alias for the `kubectl apply` command with the `-f` option. This allows you to apply YAML files to your Kubernetes cluster more easily and with fewer keystrokes.

- `export do="--dry-run=client -o yaml"`: This sets up an environment variable called `do` with some options for the `kubectl` command. Specifically, it sets the `--dry-run` option to `client` and the output format to YAML. This can be helpful for testing changes to your Kubernetes resources without actually making any changes.

- `export now="--force --grace-period 0"`: This sets up an environment variable called 'now' with some options for the `kubectl delete` command. Specifically, it sets the `--force` option to delete the resource immediately, and sets the `--grace-period` option to 0, which can be helpful for deleting resources quickly during the exam.

**Vim Setup**

Here's the contents of my `~/.vimrc` file

```arduino
set rnu nu et ai ic sts=-1 ts=2 sw=2
```

- `rnu`: This enables relative line numbering, which displays the distance between the current line and the cursor on each line. This can be useful for quickly navigating large files.

- `nu`: This enables absolute line numbering, which displays the line number of each line. This can be useful for referencing specific lines in a file.

- `et`: This sets the '`expandtab`' option, which causes Vim to insert spaces instead of tabs when you press the Tab key. This can be helpful for ensuring consistent indentation and avoiding issues with mixed tabs and spaces.

- `ai`: This sets the '`autoindent`' option, which causes Vim to automatically indent new lines to match the indentation of the previous line. This can be helpful for maintaining consistent formatting throughout a file.

- `ic`: This sets the '`ignorecase`' option, which causes Vim to ignore case when searching for text. This can be helpful for quickly finding text within a file.

- `sts=-1`: This sets the '`softtabstop`' option to -1, which causes Vim to use the value of 'shiftwidth' for tab stops. This can be helpful for ensuring consistent indentation even when using mixed indentation styles within a file.

- `ts=2`: This sets the '`tabstop`' option to 2 spaces. This determines the number of spaces that will be used for a single tab stop in the file.

- `sw=2`: This sets the '`shiftwidth`' option to 2 spaces. This determines the number of spaces that Vim will use for each level of indentation when autoindenting new lines.

