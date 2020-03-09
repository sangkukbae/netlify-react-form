# [Netlify React Form](https://www.netlify.com/blog/2017/07/20/how-to-integrate-netlifys-form-handling-in-a-react-app/#form-handling-with-a-stateless-react-form)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Contact</title>
    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
  </head>
  <body>
    <h1>Netlify React Form</h1>
    <form name="contact" netlify netlify-honeypot="bot-field" hidden>
      <input type="text" name="name" />
      <input type="email" name="email" />
      <textarea name="message"></textarea>
    </form>

    <div id="root"></div>
    <script type="text/babel">
      const encode = data => {
        return Object.keys(data)
          .map(
            key => encodeURIComponent(key) + '=' + encodeURIComponent(data[key])
          )
          .join('&');
      };

      class ContactForm extends React.Component {
        constructor(props) {
          super(props);
          this.state = { name: '', email: '', message: '' };
        }

        handleChange = e => this.setState({ [e.target.name]: e.target.value });

        handleSubmit = e => {
          fetch('/', {
            method: 'POST',
            headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
            body: encode({ 'form-name': 'contact', ...this.state })
          })
            .then(() => alert('Success!'))
            .catch(error => alert(error));

          e.preventDefault();
        };
        render() {
          const { name, email, message } = this.state;
          return (
            <form onSubmit={this.handleSubmit}>
              <p>
                <label>
                  Your Name:{' '}
                  <input
                    type="text"
                    name="name"
                    value={name}
                    onChange={this.handleChange}
                  />
                </label>
              </p>
              <p>
                <label>
                  Your Email:{' '}
                  <input
                    type="email"
                    name="email"
                    value={email}
                    onChange={this.handleChange}
                  />
                </label>
              </p>
              <p>
                <label>
                  Message:{' '}
                  <textarea
                    name="message"
                    value={message}
                    onChange={this.handleChange}
                  />
                </label>
              </p>
              <p>
                <button type="submit">Send</button>
              </p>
            </form>
          );
        }
      }

      ReactDOM.render(<ContactForm />, document.getElementById('root'));
    </script>
  </body>
</html>
```

~[https://elegant-murdock-50d78d.netlify.com](https://elegant-murdock-50d78d.netlify.com)~
