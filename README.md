# OS-Final-Project

Here’s how we did the project:

Follow installation and post-installation instructions to download Minix virtual machine
Download clang and openssh
Install nano, the superior text editor
Clone the git repository as instructed in the project specifications into the /usr/src folder
Modify the existing tty file. Steps:
cd /usr/src/minix/drivers/tty/tty
nano tty.c
add 1078 
[	if (ch == tp->tty_termios.c_cc[VSUSP]) {
		u16_t *head;
		head = tp->tty_inhead;
		//delete backwards over characters until you
		//have reached a space character (or have 
		//reached the beginning of the terminal line
		while((*--head & IN_CHAR) != ‘ ‘) {
			if (!back_over(tp)) {
				break;
			}
		}
		//delete spaces until you reach a character
		//that is not a space
		while((*head-- & IN_CHAR) == ‘ ‘) {
			(void) back_over(tp);
		}
		//decides not to print ^Z
		if (!(tp->tty_termios.c_lflag & ECHOE)) {
			(void) tty_echo(tp, ch);
		}
		continue;
	}
]

Rebuild the system:
cd /usr/src
make build
cd /usr/src/releasetools
make hdboot
reboot  (Note: hit 2 on startup)

