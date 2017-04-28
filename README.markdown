# Send a Slack message

[![Docker Automated build](https://img.shields.io/docker/automated/suhlig/slack-message.svg)](docker.io)

This is an executable script for sending messages to a Slack channel from the command line. It wraps the [slack-notifier](https://github.com/stevenosloan/slack-notifier) gem. It is also available as Docker image.

# Build

```bash
docker build --rm --force-rm -t suhlig/slack-message .
```

# Run

1. Source this Bash function:

    ```bash
    slack-message(){
      docker run                  \
          --interactive           \
          --tty                   \
          --rm                    \
          --env SLACK_WEBHOOK_URL \
        suhlig/slack-message      \
        "$@"
    }
    ```

1. Run it:

    ```bash
    slack-message --help
    ```

# Example

This command builds a colored Slack message with a title and an image of a random cat (courtesy of [thecatapi.com](http://thecatapi.com)):

```bash
# grab a random cat URL
random_cat=$(curl --write-out "%{url_effective}\n" --head --location --silent --show-error --output /dev/null http://thecatapi.com/api/images/get)

# send a Slack message with a cat
slack-message --color 'e86537' --title "Cat #$RANDOM" --image_url "$random_cat" Please have a look at this cute cat.
```

# Acknowledgements

* Thanks to [Jess Frazelle](https://github.com/jessfraz) for her [Docker files](https://github.com/jessfraz/dockerfiles) and [Docker shell functions](https://github.com/jessfraz/dotfiles/blob/master/.dockerfunc)
* [Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/45325/get-urls-redirect-target-with-curl) shows the basics or getting the redirect URL. This is necessary because Slack treats multiple uses of a single URL as the same, even if it redirects to a different place on each request (as [The Cat API](http://thecatapi.com) does).
