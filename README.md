### JSON-WEB-TOKEN-Install
> $ npm install jsonwebtoken
### Cookie Parser Install
> $ npm i cookie-parser

### Init JWT & Cookie Parser
> const jwt = require('jsonwebtoken');
> 
> const cookieParser = require('cookie-parser')

### cors Origin Change
> app.use(cors({
> 
>    origin: ['http://localhost:5173', 'http://localhost:5174'],
> 
>   credentials: true
> 
> }));

### use cookieParser(); 
> app.use(cookieParser())

### Make a post request.

        app.post('/jwt', async (req, res) => {
            const book = req.body;

            const token = jwt.sign(book, process.env.ACCESS_TOKEN_SECRET, {
                expiresIn: '1h'
            });

            res.cookie('token', token, {
                httpOnly: true,
                secure: false
            })
                .send({ success: true })

        })

### Client Side Post Setting
    axios.post('http://localhost:5000/jwt', Data, { withCredentials: true })
                    .then(res => console.log(res.data))
                    .catch(error => console.log(error))

### use maddlewars
    const verifyToken = async (req, res, next) => {
        const token = req.cookies?.token;
        if (!token) {
            return res.status(401).send({ message: 'unauthorized access' })
        }
        jwt.verify(token, process.env.ACCESS_TOKEN_SECRET, (err, decoded) => {
            if (err) {
                return res.status(401).send({ message: 'unauthorized access' })
            }
            req.user = decoded;
            next();
        })
    }

### client side axios `Credentials`
  axios('API', `{withCredentials: true}`)
