#Leev
require('dotenv').config();
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');
const helmet = require('helmet');
const cors = require('cors');
const rateLimit = require('express-rate
limit');
const cookieParser = require('cookie-parser');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const crypto = require('crypto');
const validator = require('validator');
 const app = express();
const server = http.createServer(app);
const PORT = process.env.PORT || 3000;
const IS_PROD = process.env.NODE_ENV ===
'production';
 
 ender 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

 
 Environment Variable
const JWT_SECRET = process.env.JWT_SECRET ||
crypto.randomBytes(32).toString('hex');
if (!process.env.JWT_SECRET) {
console.warn('[SECURITY WARNING] Set
JWT_SECRET in Render Environment Variables!');
}
 app.set('trust proxy', 1);

app.use((req, res, next) => {
if (IS_PROD && req.headers['x-forwarded
proto'] === 'http') {
return res.redirect(301, `https://
{req.headers.host}${req.originalUrl}`);
}
next();
});
 // 2. Security Headers (Helmet + CSP + HSTS)
app.use(helmet({
contentSecurityPolicy: {
directives: {
defaultSrc: ["'self'"],
scriptSrc: ["'self'", "'unsafe-inline'"],
scriptSrcAttr: ["'unsafe-inline'"],
styleSrc: ["'self'", "'unsafe-inline'",
"https://fonts.googleapis.com"],
fontSrc: ["'self'", "https:/
fonts.gstatic.com"],
imgSrc: ["'self'", "data:", "blob:",
"https:"],
mediaSrc: ["'self'", "data:", "blob:"],
connectSrc: ["'self'", "ws:", "wss:"],
frameAncestors: ["'none'"]
}
},
hsts: { maxAge: 31536000, includeSubDomains:
true },
referrerPolicy: { policy: 'strict-originwhen-cross-origin' }
}));
 // 3. CORS
app.use(cors({
origin: (origin, cb) => {
if (!origin) return cb(null, true);
const allowed = [
process.env.ALLOWED_ORIGIN,
'https://leev.study',
'https://www.leev.study'
].filter(Boolean);
if (!IS_PROD ||
origin.endsWith('.onrender.com') ||
allowed.includes(origin)) {
return cb(null, true);
}
return cb(new Error('CORS_BLOCKED'));
},
credentials: true
}));
 app.use(express.json({ limit: '2mb' }));
app.use(cookieParser());
 // 4. Rate Limiting
const apiLimiter = rateLimit({
windowMs: 15 * 60 * 1000,
max: 300,
message: { error: 'ط
 
 
 
 
 
 
 ً
 
 
 
 
 
 
 
 
 
 
 
 
 
اً.' 
 
 
 
 }});
const authLimiter = rateLimit({
windowMs: 15 * 60 * 1000,
max: 10,
message: { error: 'م
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
اً.' 
 
 
 
 
 
 
 
 
 
 
 
 }
});
app.use('/api/', apiLimiter);
 
 
 
 
 
 
 
 
 
 
 
 
 
 ُ
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ostgreSQL/Supabas
 

 
 
 
 
 
 
 
 
const db = {
settings: {
aiEnabled: true,
announcement: 'م
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ً
 
 
 
ة 
 
 
 
 
 
 
 
 LEEV.study — ن
 س!'
 
 
 
 
 
 
 
 
 
},
users: [],
homework: [
{
id: 'hw_1',
subject: 'ا
 ت',
 
 
 
 
 
 
page: 'ص
 5',
 
 
 2
 
 
 
 
 
 
 
 
 45
 
 
 
due: 'غ
 اً',
 
 
 
 
 8:00
 
 
 
 
 
 
 
 ً
 
notes: 'ا
 ت.',
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
author: 'ا
 ي',
 
 
 
 
 
 
 
 
 
 
completedBy: []
}
],
books: [{ id: 'b_1', title: 'ك
 ل',
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
subject: 'ا
 ت', 
 
 
 
 
 
 url: 'https://ien.edu.sa/'
},
{ id: 'b_2', title: 'م
 
 
 
 
 
 
 
 
 
 
 
 
 
ي', 
 
 
 
 
 
 
 subject: 'ا
 ء', 
 
 
 
 
 url: 'https:/
phet.colorado.edu/ar/' }
],
links: [
{ id: 'l_1', title: 'ب
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
)', 
 
 
 
 url: 'https://ien.edu.sa/' }
],
stories: [
{ id: 'st_1', user: 'م
 ف', 
 
 
 
 
 avatar:
'🍃', text: 'أ
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ً
 
 
يك 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 🔥', createdAt: Date.now()
}
],
rooms: {
'public-general': [
{ id: 'm1', sender: 'م
 ف', 
 
 
 
 
 type:
'text', content: 'م
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ً
 
 
 
ة.', 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 time:
'ا
 آن', disappear: false }
],
'public-homework': [
{ id: 'm2', sender: 'م
 ف', 
 
 
 
 
 type:
'text', content: 'ه
 س.',
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
time: 'ا
 آن', disappear: false }
]
},
securityLogs: [],failedAttempts: new Map(),
blockedIPs: new Map()
};
 
 nvironment 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
ender
 
 
 
 
 
 Variable
(async () => {
let ownerHash =
process.env.OWNER_PASSWORD_HASH;
if (!ownerHash || 
ownerHash.startsWith('$2')) {
const rawPass = process.env.OWNER_PASSWORD
|| 'ChangeMe_LEEV_2025!';
ownerHash = await bcrypt.hash(rawPass, 12);
if (!process.env.OWNER_PASSWORD && 
process.env.OWNER_PASSWORD_HASH) {
console.warn('[OWNER] Set OWNER_PASSWORD
or OWNER_PASSWORD_HASH in Render Environment
Variables!');
}
}
db.users.push({
id: 'owner_official',
name: 'ا
 ي',
 
 
 
 
 
 
 
 
 
 
username: (process.env.OWNER_USERNAME ||
'leev_owner').toLowerCase(),
email: (process.env.OWNER_EMAIL ||
'owner@leev.study').toLowerCase(),
passwordHash: ownerHash,
passwordUpdatedAt: Date.now(),
role: 'owner',approved: true,
avatar: '👑',
bio: 'م
 ة',
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
xp: 999,
streak: 1,
friends: [],
notes: '',
lockedUntil: 0
});
})();
 function logSecurity(type, ip, detail, severity
= 'WARN') {
db.securityLogs.unshift({
id: crypto.randomUUID(),
time: new Date().toLocaleString('ar-SA'),
type,
ip,
detail: String(detail).slice(0, 180),
severity
});
if (db.securityLogs.length > 200)
db.securityLogs.pop();
if (severity === 'CRITICAL') {
db.blockedIPs.set(ip, Date.now() + 20 * 60
* 1000);
}
}
 function sanitize(str, max = 300) {
if (typeof str !== 'string') return '';return validator.escape(str.trim()).slice(0,
max);
}
 function safeHref(url) {
try {
const u = new URL(url);
return (u.protocol === 'https:' ||
u.protocol === 'http:') ? u.href : '#';
} catch { return '#'; }
}
 function isHttpsRequest(req) {
return req.secure || req.headers['x
forwarded-proto'] === 'https';
}
 
 
 
 
 
 
 
 
 
 
 
 
 
app.use((req, res, next) => {
const until = db.blockedIPs.get(req.ip);
if (until && Date.now() < until) {
return res.status(403).json({ error: 'ت
 
ه.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ً
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
next();
});
 
 SRF
 
 
 
 
 
 
 
 
 
 
 
 
app.get('/api/csrf-token', (req, res) => {
const token =
crypto.randomBytes(24).toString('hex');res.cookie('leev_csrf', token, {
httpOnly: false,
secure: isHttpsRequest(req),
sameSite: 'strict',
maxAge: 3600 * 1000
});
res.json({ csrfToken: token });
});
 app.use('/api/', (req, res, next) => {
if (['GET', 'HEAD',
'OPTIONS'].includes(req.method)) return next();
const c = req.cookies.leev_csrf;
const h = req.headers['x-csrf-token'];
if (!c || !h || c !== h) {
logSecurity('CSRF_BLOCKED', req.ip,
req.originalUrl, 'WARN');
return res.status(403).json({ error: 'ف
 
 
ني 
 
 
 
 
 
 
 
 
 
 
 (CSRF). أ
 ة.' 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
next();
});
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
function requireAuth(req, res, next) {
const token = req.cookies.leev_session;
if (!token) return res.status(401).json({
error: 'ي
 ل.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
try {
const payload = jwt.verify(token,
JWT_SECRET);if (payload.role === 'demo') {
req.user = { id: 'demo_viewer', name:
'ط
 )', 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 username: 'demo',
role: 'demo', avatar: '👁️', xp: 0, streak: 1,
friends: [], notes: '' };
return next();
}
const user = db.users.find(u => u.id ===
payload.uid);
if (!user || !user.approved) return
res.status(401).json({ error: 'ح
 ح.'
 
 
 
 
 
 
 
 
 
 
});
const NINETY_DAYS = 90 * 24 * 60 * 60 *
1000;
if (Date.now() - user.passwordUpdatedAt >
NINETY_DAYS) {
return res.status(403).json({ error:
'ا
 ً).' 
 
 
 
 (90
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
req.user = user;
next();
} catch {
return res.status(401).json({ error: 'ا
 
 
 
 
ل.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
}
 function rejectDemo(req, res, next) {
if (req.user?.role === 'demo') {
return res.status(403).json({ error: 'و
 
 
ط.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });}
next();
}
 function requireOwner(req, res, next) {
if (!req.user || req.user.role !== 'owner') {
logSecurity('UNAUTHORIZED_ADMIN_ACCESS',
req.ip, `User ${req.user?.username ||
'unknown'} hit ${req.originalUrl}`,
'CRITICAL');
res.clearCookie('leev_session');
return res.status(403).json({ error: 'م
 
 
 
 
 
تك.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
next();
}
 function publicUser(u) {
return {
id: u.id, name: u.name, username:
u.username, role: u.role,
avatar: u.avatar, bio: u.bio || '', xp:
u.xp || 0, streak: u.streak || 1,
friends: u.friends || [], notes: u.notes ||
''
};
}
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
app.post('/api/auth/register', authLimiter,
async (req, res) => {const { name, username, email, password } =
req.body || {};
if (!name || typeof name !== 'string' ||
name.trim().length < 2) {
return res.status(400).json({ error: 'ا
 
 
 
 
لح.' 
 
 
 
 
 
 });
}
if (!username || !/^[a-zA-Z0-9_]{3,20
$/.test(username)) {
return res.status(400).json({ error: 'ا
 
20).'
 (3
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
});
}
if (!email || !validator.isEmail(email)) {
return res.status(400).json({ error:
'ا
 يح.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
if (!password || 
validator.isStrongPassword(password, {
minLength: 8, minLowercase: 1, minUppercase: 1,
minNumbers: 1, minSymbols: 0 })) {
return res.status(400).json({ error: 'ك
 
 
).'
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 8
 
 
 
 
 
 
 
 
 
 
 
 
 
 
});
}
  const cleanEmail =
validator.normalizeEmail(email).toLowerCase();
const cleanUser = username.toLowerCase();
if (db.users.some(u => u.email === cleanEmail
|| u.username === cleanUser)) {return res.status(409).json({ error: 'ا
 
 
 
 
 
اً.' 
 
 
 
 
 
 
 
 
 });
}
  const passwordHash = await
bcrypt.hash(password, 12);
db.users.push({
id: crypto.randomUUID(),
name: sanitize(name, 60),
username: cleanUser,
email: cleanEmail,
passwordHash,
passwordUpdatedAt: Date.now(),
role: 'student',
approved: false,
avatar: '🎓',
bio: 'ط
 ف',
 
 
 
 
 
 
 
 
 
 
 
 
 
xp: 10,
streak: 1,
friends: [],
notes: '',
lockedUntil: 0
});
res.status(201).json({ message: 'ت
 
 
 
 
 
 
 
 
 
 
 
ه.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
});
 app.post('/api/auth/login', authLimiter, async
(req, res) => {
const { identifier, password } = req.body ||
{};if (!identifier || !password || typeof
identifier !== 'string' || typeof password !==
'string') {
return res.status(400).json({ error: 'أ
 
 
 
ة.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
const key = identifier.trim().toLowerCase();
const user = db.users.find(u => u.email ===
key || u.username === key);
if (!user) {
logSecurity('FAILED_LOGIN', req.ip,
`Unknown: ${key}`);
return res.status(401).json({ error: 'ب
 
 
 
 
 
ة.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
if (user.lockedUntil > Date.now()) {
return res.status(423).json({ error: 'ا
 
 
 
 
 
لة.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ً
 
 
 
 
 
 
 
 
 
 
 });
}
const ok = await bcrypt.compare(password,
user.passwordHash);
if (!ok) {
const c = (db.failedAttempts.get(user.id)
|| 0) + 1;
db.failedAttempts.set(user.id, c);
logSecurity('FAILED_LOGIN', req.ip, `Wrong
pass for ${user.username} (${c}/5)`, c >= 5 ?
'CRITICAL' : 'WARN');
if (c >= 5) {
user.lockedUntil = Date.now() + 15 * 60 *
1000;db.failedAttempts.delete(user.id);
}
return res.status(401).json({ error: 'ب
 
 
 
 
 
ة.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
if (!user.approved) {
return res.status(403).json({ error: 'ح
 
 
 
 
ي.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
db.failedAttempts.delete(user.id);
  const token = jwt.sign({ uid: user.id, role:
user.role }, JWT_SECRET, { expiresIn: '4h' });
res.cookie('leev_session', token, {
httpOnly: true,
secure: isHttpsRequest(req),
sameSite: 'strict',
maxAge: 4 * 60 * 60 * 1000
});
res.json({ user: publicUser(user) });
});
 app.post('/api/auth/demo', (req, res) => {
const token = jwt.sign({ uid: 'demo_viewer',
role: 'demo' }, JWT_SECRET, { expiresIn: '2h'
});
res.cookie('leev_session', token, {
httpOnly: true,
secure: isHttpsRequest(req),
sameSite: 'strict',
maxAge: 2 * 60 * 60 * 1000});
res.json({ user: { id: 'demo_viewer', name:
'ط
 )', 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 username: 'demo',
role: 'demo', avatar: '👁️', xp: 0, streak: 1,
friends: [], notes: '' } });
});
 app.post('/api/auth/logout', (req, res) => {
res.clearCookie('leev_session');
res.json({ ok: true });
});
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
app.get('/api/bootstrap', requireAuth, (req,
res) => { 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
const dayAgo = Date.now() - 24 * 60 * 60 *
1000;
db.stories = db.stories.filter(s =>
s.createdAt > dayAgo);
  const friendsList = (req.user.friends ||
[]).map(uname => {
const f = db.users.find(u => u.username ===
uname && u.approved);
return f ? { name: f.name, username:
f.username, avatar: f.avatar, xp: f.xp, streak:
f.streak } : null;
}).filter(Boolean);
  res.json({user: publicUser(req.user),
settings: db.settings,
homework: db.homework,
books: db.books,
links: db.links,
stories: db.stories,
friendsList
});
});
 app.post('/api/homework', requireAuth,
rejectDemo, (req, res) => {
if (req.user.role !== 'owner' &&
req.user.role !== 'teacher') {
return res.status(403).json({ error: 'ص
قط.' 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
const { subject, page, due, notes } =
req.body || {};
if (!subject || !page || !due) return
res.status(400).json({ error: 'أ
 
 
 
 
 
 
 
 
ب.' 
 
 
 
 
 });
const hw = {
id: 'hw_' + Date.now(),
subject: sanitize(subject, 80),
page: sanitize(page, 120),
due: sanitize(due, 80),
notes: sanitize(notes || '', 240),
author: req.user.name,
completedBy: []
};
 
 
 
 
db.homework.unshift(hw);
io.emit('new-homework', hw); // إ
 
 
 
 
 
 
 
 
 
  
 
 res.json({ 
 
 
 
 
 
 
 
 
 
 
 homework: 
 
 
 
 
 
 
 db.homework });
});
 app.post('/api/homework/:id/toggle',
requireAuth, rejectDemo, (req, res) => {
const hw = db.homework.find(h => h.id ===
req.params.id);
if (!hw) return res.status(404).json({ error:
'غ
 د.' 
 
 
 
 
 
 });
const idx =
hw.completedBy.indexOf(req.user.id);
if (idx === -1) {
hw.completedBy.push(req.user.id);
req.user.xp += 15;
} else {
hw.completedBy.splice(idx, 1);
}
res.json({ homework: db.homework, xp:
req.user.xp });
});
 app.delete('/api/homework/:id', requireAuth,
rejectDemo, (req, res) => {
if (req.user.role !== 'owner' &&
req.user.role !== 'teacher') return
res.status(403).json({ error: 'م
 ع.' 
 
 });
db.homework = db.homework.filter(h => h.id 
== req.params.id);res.json({ homework: db.homework });
});
 app.post('/api/stories', requireAuth,
rejectDemo, (req, res) => {
const text = sanitize(req.body?.text, 120);
if (!text) return res.status(400).json({
error: 'ا
 غ.' 
 
 
 
 
 
 });
db.stories.unshift({ id: 'st_' + Date.now(),
user: req.user.name, avatar: req.user.avatar,
text, createdAt: Date.now() });
req.user.streak = (req.user.streak || 1) + 1;
req.user.xp += 10;
res.json({ stories: db.stories, streak:
req.user.streak, xp: req.user.xp });
});
 app.post('/api/friends/add', requireAuth,
rejectDemo, (req, res) => {
const uname = String(req.body?.username ||
'').trim().toLowerCase().replace('@', '');
if (!uname || uname === req.user.username)
return res.status(400).json({ error: 'ا
 
 
 
 
 
 
 
 
 
ح.' 
 
 
 
 
 
 
 });
const target = db.users.find(u => u.username
=== uname && u.approved);
if (!target) return res.status(404).json({
error: 'ل
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
م.' 
 
 
 
 
 
 
 
 });
if (!req.user.friends.includes(uname))
req.user.friends.push(uname);res.json({ ok: true, targetName: target.name
});
});
 app.post('/api/notes', requireAuth, rejectDemo,
(req, res) => {
req.user.notes = String(req.body?.notes ||
'').slice(0, 5000);
res.json({ ok: true });
});
 app.post('/api/profile', requireAuth,
rejectDemo, (req, res) => {
const name = sanitize(req.body?.name, 60);
const bio = sanitize(req.body?.bio || '',
120);
const allowedAvatars = ['🎓', '🚀', '🦁', '⚡',
'🦉', '👑'];
if (name.length >= 2) req.user.name = name;
req.user.bio = bio;
if
(allowedAvatars.includes(req.body?.avatar))
req.user.avatar = req.body.avatar;
res.json({ user: publicUser(req.user) });
});
 app.post('/api/ai/ask', requireAuth,
rejectDemo, async (req, res) => {
if (!db.settings.aiEnabled) return
res.status(403).json({ error: 'ا
 
 
 
 
 
 
 
 
 
 
 
 
ك.' 
 
 
 
 
 
 
 
 
 
 
 
 
 });const q = sanitize(req.body?.question, 500);
if (!q) return res.status(400).json({ error:
'ا
 اً.' 
 
 
 
 
 
 
 });
 
 
 
 
 EMINI_API_KE
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 emin
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ende
 

 
 
 
 
 
 
 
 
 
 
 
 
if (process.env.GEMINI_API_KEY) {
try {
const r = await fetch(`https:/
generativelanguage.googleapis.com/v1beta
models/gemini-1.5-flash:generateContent?key=
{process.env.GEMINI_API_KEY}`, {
method: 'POST',
headers: { 'Content-Type':
'application/json' },
body: JSON.stringify({ contents: [{
parts: [{ text: `أ
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
ح: 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ${q}` }]
}] })
});
const d = await r.json();
const reply = d?.candidates?
[0]?.content?.parts?.[0]?.text;
if (reply) return res.json({ reply });
} catch {}
}
  res.json({ reply: `✨ ب
 ك 
 
 
 
 
 
 
 
 "${q}": ق
 ّ

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ّ
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
ة 
 
 
 
 
 
 
 
 ً
 
 
 
 
 
 
 
 
 
 
 
 Gemini أو NotebookLMمق.` 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
 
});
 
 
 
 dmin Lockou
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
app.get('/api/admin/panel', requireAuth,
requireOwner, (req, res) => {
res.json({
pending: db.users.filter(u => 
u.approved).map(u => ({ id: u.id, name: u.name,
username: u.username, email: u.email })),
approved: db.users.filter(u =>
u.approved).map(u => ({ id: u.id, name: u.name,
username: u.username, role: u.role, avatar:
u.avatar })),
securityLogs: db.securityLogs
});
});
 app.post('/api/admin/approve', requireAuth,
requireOwner, (req, res) => {
const u = db.users.find(x => x.id ===
req.body?.userId);
if (!u) return res.status(404).json({ error:
'غ
 د.' 
 
 
 
 
 
 });
u.approved = true;
res.json({ ok: true });
});
 app.post('/api/admin/reject', requireAuth,
requireOwner, (req, res) => {
db.users = db.users.filter(x => x.id !==req.body?.userId || x.role === 'owner');
res.json({ ok: true });
});
 app.post('/api/admin/role', requireAuth,
requireOwner, (req, res) => {
const { userId, role } = req.body || {};
if (!['student', 'teacher'].includes(role))
return res.status(400).json({ error: 'ص
 
 
 
 
 
 
 
 
 
حة.' 
 
 
 });
const u = db.users.find(x => x.id ===
userId);
if (!u || u.role === 'owner') return
res.status(403).json({ error: 'غ
 ح.' 
 
 
 
 
 
 });
u.role = role;
res.json({ ok: true });
});
 app.post('/api/admin/settings', requireAuth,
requireOwner, (req, res) => {
if (typeof req.body?.aiEnabled === 'boolean')
db.settings.aiEnabled = req.body.aiEnabled;
if (typeof req.body?.announcement ===
'string') db.settings.announcement =
sanitize(req.body.announcement, 200);
io.emit('settings-updated', db.settings);
res.json({ settings: db.settings });
});
 app.post('/api/admin/books', requireAuth,
requireOwner, (req, res) => {const { title, subject, url } = req.body ||
{};
if (!title || !subject) return
res.status(400).json({ error: 'ب
 ة.'
 
 
 
 
 
 
 
 
 
});
db.books.unshift({ id: 'b_' + Date.now(),
title: sanitize(title, 100), subject:
sanitize(subject, 60), url: safeHref(url ||
'https://ien.edu.sa/') });
res.json({ books: db.books });
});
 app.delete('/api/admin/books/:id', requireAuth,
requireOwner, (req, res) => {
db.books = db.books.filter(b => b.id !==
req.params.id);
res.json({ books: db.books });
});
 app.post('/api/admin/links', requireAuth,
requireOwner, (req, res) => {
const { title, url } = req.body || {};
if (!title || !url) return
res.status(400).json({ error: 'ب
 ة.'
 
 
 
 
 
 
 
 
 
});
db.links.unshift({ id: 'l_' + Date.now(),
title: sanitize(title, 100), url: safeHref(url)
});
res.json({ links: db.links });
});
// 11. Socket.IO ل
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 const 
 
 
 
 
 
 io 
 
 
 = 
 
new 
 
 
 
 Server(server, { cors: { origin:
true, credentials: true } });
 io.use((socket, next) => {
try {
const raw = socket.handshake.headers.cookie
|| '';
const match = raw.match(/leev_session=([^;
+)/);
if (!match) return next(new
Error('UNAUTHORIZED'));
const payload = jwt.verify(match[1],
JWT_SECRET);
if (payload.role === 'demo') {
socket.user = { id: 'demo', name: 'ط
 
 
 
)', 
 
 
 
 
 
 
 
 
 
 
 
 
 
 role: 'demo' };
return next();
}
const user = db.users.find(u => u.id ===
payload.uid && u.approved);
if (!user) return next(new
Error('UNAUTHORIZED'));
socket.user = { id: user.id, name:
user.name, username: user.username, role:
user.role };
next();
} catch {
next(new Error('UNAUTHORIZED'));
}});
 io.on('connection', (socket) => {
socket.on('join-room', (roomId) => {
const clean = String(roomId || 'public
general').replace(/[^a-zA-Z0-9_-]/g,
'').slice(0, 40);
socket.join(clean);
if (!db.rooms[clean]) db.rooms[clean] = [];
socket.emit('room-history', { roomId:
clean, messages: db.rooms[clean].slice(-50) });
socket.to(clean).emit('user-joined-voice',
{ socketId: socket.id, name: socket.user.name
});
});
  socket.on('chat-message', ({ roomId, type,
content, data, disappear }) => {
if (socket.user.role === 'demo') return;
const cleanRoom = String(roomId || 'public
general').replace(/[^a-zA-Z0-9_-]/g,
'').slice(0, 40);
const safeType = ['text', 'image',
'audio'].includes(type) ? type : 'text';
let safeData = null;
if (safeType === 'image' && typeof data ===
'string' && data.startsWith('data:image/') &&
data.length < 1800000) safeData = data;
if (safeType === 'audio' && typeof data ===
'string' && data.startsWith('data:audio/') &&
data.length < 1800000) safeData = data;  const msg = {
id: crypto.randomUUID(),
sender: socket.user.name,
type: safeType,
content: sanitize(String(content || ''),
500),
data: safeData,
disappear: Boolean(disappear),
time: new Date().toLocaleTimeString('ar
SA', { hour: '2-digit', minute: '2-digit' })
};
if (!db.rooms[cleanRoom])
db.rooms[cleanRoom] = [];
db.rooms[cleanRoom].push(msg);
if (db.rooms[cleanRoom].length > 80)
db.rooms[cleanRoom].shift();
  io.to(cleanRoom).emit('chat-message', {
roomId: cleanRoom, msg });
  if (msg.disappear) {
setTimeout(() => {
db.rooms[cleanRoom] =
(db.rooms[cleanRoom] || []).filter(x => x.id 
== msg.id);
io.to(cleanRoom).emit('delete-message',
{ roomId: cleanRoom, msgId: msg.id });
}, 9000);
}
});  // WebRTC Signaling ل
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
 
 socket.on('webrtc-signal', 
 
 
 
 
 
 ({ targetId,
signal }) => {
io.to(targetId).emit('webrtc-signal', {
fromId: socket.id, name: socket.user.name,
signal });
});
});
 
 
 
 E
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
app.get('/robots.txt', (req, res) => {
res.type('text/plain').send('User-agent:
*\nAllow: /\nSitemap: https://' +
req.headers.host + '/sitemap.xml\n');
});
 app.get('/sitemap.xml', (req, res) => {
res.type('application/xml').send('<?xml
version="1.0" encoding="UTF-8"?><urlset
xmlns="http://www.sitemaps.org/schemas/sitemap
0.0"><url><loc>https://' + req.headers.host +
'/</loc><priority>1.0</priority></url><
urlset>');
});
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
const HTML_PAGE = `<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head><meta charset="UTF-8" />
<meta name="viewport" content="width=device
width, initial-scale=1.0" />
<title>م
 ة 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 | LEEV.study</title>
<meta name="description" content="م
 
 
 
 
 
 
 
ة 
 
 
 
 
 
 
 
 LEEV.study - و
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
اب." 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 />
<meta name="keywords" content="م
 ف, 
 
 
 
 
 LEEV,
LEEV.study, م
 ت" 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 />
<link rel="preconnect" href="https:/
fonts.googleapis.com">
<link rel="preconnect" href="https:/
fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com
css2
family=Cairo:wght@400;600;700;800;900&display=
wap" rel="stylesheet">
<style>
:root {
--bg-gradient: linear-gradient(135deg,
#f8fafc 0%, #eef2ff 50%, #e0f2fe 100%);
--card-bg: rgba(255, 255, 255, 0.94);
--card-border: rgba(15, 23, 42, 0.09);
--sidebar-bg: rgba(255, 255, 255, 0.96);
--input-bg: #f1f5f9;
--accent: #4f46e5;
--accent-2: #0284c7;
--accent-glow: rgba(79, 70, 229, 0.22);
--text-main: #0f172a;
--text-muted: #475569;
--success: #10b981;--warning: #f59e0b;
--danger: #ef4444;
--chat-in: #e2e8f0;
}
body[data-mode="dark"] {
--bg-gradient: linear-gradient(135deg,
#111827 0%, #1e293b 55%, #172554 100%);
--card-bg: rgba(30, 41, 59, 0.88);
--card-border: rgba(255, 255, 255, 0.1);
--sidebar-bg: rgba(17, 24, 39, 0.94);
--input-bg: rgba(15, 23, 42, 0.65);
--accent: #6366f1;
--accent-2: #38bdf8;
--accent-glow: rgba(99, 102, 241, 0.3);
--text-main: #f1f5f9;
--text-muted: #94a3b8;
--chat-in: rgba(255, 255, 255, 0.08);
}
* { box-sizing: border-box; margin: 0;
padding: 0; font-family: 'Cairo', system-ui,
sans-serif; }
body { background: var(--bg-gradient);
background-attachment: fixed; color: var(-
text-main); min-height: 100vh; overflow-x:
hidden; transition: 0.25s; }
.auth-wrapper { min-height: 100vh; display:
flex; align-items: center; justify-content:
center; padding: 20px; }
.auth-card { width: 100%; max-width: 470px;
background: var(--card-bg); border: 1px solid
var(--card-border); border-radius: 24px;padding: 28px; box-shadow: 0 20px 50px
rgba(0,0,0,0.12); }
.brand-logo { display: flex; align-items:
center; gap: 10px; font-weight: 900; font-size:
1.25rem; }
.domain-pill { display: inline-block; font
size: 0.74rem; padding: 2px 9px; border-radius:
99px; background: rgba(79,70,229,0.12); color:
var(--accent); border: 1px solid
rgba(79,70,229,0.25); font-weight: 800;
direction: ltr; }
.form-group { margin-bottom: 12px; }
.form-label { display: block; font-size:
0.84rem; color: var(--text-muted); margin
bottom: 5px; font-weight: 700; }
.input, .select, .textarea { width: 100%;
padding: 10px 14px; border-radius: 12px;
border: 1px solid var(--card-border);
background: var(--input-bg); color: var(--text
main); font-size: 0.93rem; outline: none; }
.input:focus, .textarea:focus { border
color: var(--accent); box-shadow: 0 0 0 3px
var(--accent-glow); }
.btn { display: inline-flex; align-items:
center; justify-content: center; gap: 6px;
padding: 9px 15px; border-radius: 12px; border:
none; cursor: pointer; font-weight: 700; font
size: 0.9rem; transition: 0.18s; text
decoration: none; }
.btn-primary { background: linear
gradient(135deg, var(--accent), var(-accent-2)); color: #fff; }
.btn-ghost { background:
rgba(148,163,184,0.15); color: var(--text
main); border: 1px solid var(--card-border); }
.btn-success { background: var(--success);
color: #fff; }
.btn-danger { background: var(--danger);
color: #fff; }
.btn-sm { padding: 6px 11px; font-size:
0.8rem; border-radius: 9px; }
  .app-shell { display: flex; min-height:
100vh; width: 100%; }
.sidebar { width: 260px; background: var(-
sidebar-bg); border-left: 1px solid var(--card
border); padding: 18px 12px; display: flex;
flex-direction: column; position: sticky; top:
0; height: 100vh; z-index: 40; transition:
transform 0.3s ease; }
.nav-list { margin-top: 18px; display:
flex; flex-direction: column; gap: 5px; flex:
1; overflow-y: auto; }
.nav-item { display: flex; align-items:
center; gap: 10px; padding: 10px 13px; border
radius: 12px; color: var(--text-muted); cursor:
pointer; font-weight: 700; font-size: 0.91rem;
}
.nav-item:hover { background:
rgba(99,102,241,0.08); color: var(--text-main);
}
.nav-item.active { background: lineargradient(135deg, var(--accent), var(-
accent-2)); color: #fff; }
.nav-badge { margin-right: auto;
background: var(--danger); color: #fff; font
size: 0.72rem; padding: 2px 7px; border-radius:
99px; }
  .main-area { flex: 1; display: flex; flex
direction: column; min-width: 0; }
.topbar { padding: 12px 22px; display:
flex; align-items: center; justify-content:
space-between; gap: 10px; flex-wrap: wrap;
border-bottom: 1px solid var(--card-border);
background: var(--sidebar-bg); position:
sticky; top: 0; z-index: 30; }
.content-area { padding: 22px 24px; width:
100%; max-width: 1560px; margin: 0 auto; }
.glass-card { background: var(--card-bg);
border: 1px solid var(--card-border); border
radius: 18px; padding: 18px; box-shadow: 0 8px
24px rgba(0,0,0,0.06); }
.grid-2 { display: grid; grid-template
columns: repeat(2, minmax(0, 1fr)); gap: 16px;
}
.grid-3 { display: grid; grid-template
columns: repeat(3, minmax(0, 1fr)); gap: 16px;
}
.grid-4 { display: grid; grid-template
columns: repeat(4, minmax(0, 1fr)); gap: 14px;
}
.pill { display: inline-flex; align-items:center; gap: 5px; padding: 4px 10px; border
radius: 99px; font-size: 0.76rem; font-weight:
700; }
.pill-accent { background:
rgba(79,70,229,0.12); color: var(--accent); }
.pill-success { background:
rgba(16,185,129,0.15); color: #059669; }
.pill-warning { background:
rgba(245,158,11,0.16); color: #d97706; }
.pill-danger { background:
rgba(239,68,68,0.15); color: #dc2626; }
body[data-mode="dark"] .pill-success {
color: #6ee7b7; }
body[data-mode="dark"] .pill-warning {
color: #fcd34d; }
body[data-mode="dark"] .pill-danger {
color: #fca5a5; }
  .social-layout { display: grid; grid
template-columns: 265px 1fr; gap: 16px; }
.channel-btn { display: flex; align-items:
center; justify-content: space-between; width:
100%; padding: 9px 12px; border-radius: 10px;
border: 1px solid transparent; background:
transparent; color: var(--text-muted); cursor:
pointer; font-weight: 700; font-size: 0.88rem;
text-align: right; }
.channel-btn:hover, .channel-btn.active {
background: rgba(79,70,229,0.12); color: var(-
accent); border-color: rgba(79,70,229,0.25); }
.chat-messages { height: 340px; overflow-y:auto; padding: 14px; border-radius: 14px;
background: var(--input-bg); border: 1px solid
var(--card-border); display: flex; flex
direction: column; gap: 10px; }
.msg-bubble { max-width: 78%; padding: 10px
14px; border-radius: 14px; font-size: 0.9rem;
line-height: 1.5; }
.msg-mine { align-self: flex-start;
background: linear-gradient(135deg, var(-
accent), var(--accent-2)); color: #fff; }
.msg-other { align-self: flex-end;
background: var(--chat-in); }
.stories-row { display: flex; gap: 12px;
overflow-x: auto; padding-bottom: 6px; }
.story-circle { min-width: 74px; text
align: center; cursor: pointer; }
.story-ring { width: 56px; height: 56px;
border-radius: 50%; margin: 0 auto 4px;
padding: 3px; background: linear
gradient(135deg, #f59e0b, #ec4899, #6366f1);
display: grid; place-items: center; }
.story-inner { width: 100%; height: 100%;
border-radius: 50%; background: var(--card-bg);
display: grid; place-items: center; font-size:
1.35rem; }
  .pomodoro-circle { width: 205px; height:
205px; border-radius: 50%; margin: 14px auto;
display: flex; align-items: center; justify
content: center; background: conic
gradient(var(--accent) var(--progress, 100%),rgba(148,163,184,0.2) 0%); }
.pomodoro-inner { width: 180px; height:
180px; border-radius: 50%; background: var(-
card-bg); display: flex; flex-direction:
column; align-items: center; justify-content:
center; }
.pomodoro-time { font-size: 2.6rem; font
weight: 900; direction: ltr; }
#whiteboardCanvas { width: 100%; height:
430px; border-radius: 14px; border: 1px solid
var(--card-border); cursor: crosshair; touch
action: none; }
  .mobile-menu-btn { display: none; }
.sidebar-overlay { display: none; }
@media (max-width: 1050px) { .grid-4,
.grid-3 { grid-template-columns: repeat(2,
1fr); } .social-layout { grid-template-columns:
1fr; } }
@media (max-width: 820px) {
.sidebar { position: fixed; right: 0;
top: 0; transform: translateX(100%); }
.sidebar.open { transform: translateX(0);
}
.sidebar-overlay.open { display: block;
position: fixed; inset: 0; background:
rgba(0,0,0,0.5); z-index: 35; }
.mobile-menu-btn { display: inline-flex;
}
.grid-2, .grid-3, .grid-4 { grid
template-columns: 1fr; }.content-area { padding: 14px; }
}
.hidden { display: none !important; }
.toast { position: fixed; bottom: 22px;
left: 22px; z-index: 999; background: #0f172a;
color: #fff; border: 1px solid var(--accent);
padding: 11px 18px; border-radius: 12px; font
weight: 700; box-shadow: 0 10px 25px
rgba(0,0,0,0.25); }
</style>
</head>
<body>
<!-- AUTH SCREEN -->
<div id="authScreen" class="auth-wrapper">
<div class="auth-card">
<div style="display:flex; justify
content:space-between; align-items:center;
margin-bottom:20px;">
<div class="brand-logo">
<svg width="42" height="42"
viewBox="0 0 48 48" fill="none">
<rect width="48" height="48"
rx="14" fill="url(#leevG1)"/>
<path d="M15 32C15 21 23 14 34
14C34 25 27 32 15 32Z" fill="white"/>
<defs><linearGradient id="leevG1"
x1="0" y1="0" x2="48" y2="48"><stop stop
color="#4F46E5"/><stop offset="1" stop
color="#06B6D4"/></linearGradient></defs>
</svg>
<div><div>م
 ف 
 
 
 
 
 LEEV</div><spanclass="domain-pill">LEEV.study</span></div>
</div>
<select id="authThemeSelect"
class="select" style="width:auto; padding:6px
10px; font-size:0.82rem;"
onchange="setThemePreference(this.value)">
<option value="auto">🌓 ح
 ز<
 
 
 
 
 
 
 
option>
<option value="light">☀️ ف
 ح<
 
 
 
 
 
 
option>
<option value="dark">🌙 غ
 ئ<
 
 
 
 
 
 
option>
</select>
</div>
  <div style="display:flex; gap:8px;
margin-bottom:18px;">
<button class="btn btn-primary"
style="flex:1;" id="tabLoginBtn"
onclick="switchAuthTab('login')">ت
 ل<
 
 
 
 
 
 
 
 
 
button>
<button class="btn btn-ghost"
style="flex:1;" id="tabRegBtn"
onclick="switchAuthTab('register')">ح
 د<
 
 
 
 
 
 
button>
</div>
  <form id="loginForm"
onsubmit="handleLogin(event)">
<div class="form-group"><label
class="form-label">ا
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
دم</label><input 
 
 
 
 
 
 type="text"
id="loginIdentifier" class="input" required 
></div>
<div class="form-group"><label
class="form-label">ك
 ر</label><input
 
 
 
 
 
 
 
 
type="password" id="loginPassword"
class="input" required /></div>
<button type="submit" class="btn btn
primary" style="width:100%; padding:11px;">د
 
 
 
ن 
 
 🔒</button>
<div style="margin-top:14px; padding
top:14px; border-top:1px dashed var(--card
border);">
<button type="button" class="btn btn
ghost btn-sm" style="width:100%;"
onclick="enterDemoStudent()">👁️ د
 
 
 
 
 
 
 
 
 
)</button>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
</div>
</form>
  <form id="registerForm" class="hidden"
onsubmit="handleRegister(event)">
<div class="form-group"><label
class="form-label">ا
 مل</label><input
 
 
 
 
 
 
 
 
type="text" id="regName" class="input" required
/></div>
<div class="form-group"><label
class="form-label">ا
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
)</label><input 
 
 
 
 
 
 type="text"
id="regUsername" class="input" dir="ltr"
pattern="[A-Za-z0-9_]{3,20}" required /></div><div class="form-group"><label
class="form-label">ا
 ني<
 
 
 
 
 
 
 
 
 
 
 
 
 
label><input type="email" id="regEmail"
class="input" dir="ltr" required /></div>
<div class="form-group"><label
class="form-label">ك
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 8
 
 
 
 
 
 
 
 
 
 
 
)</label><input 
 
 
 
 
 
 
 
 
 
 type="password"
id="regPassword" class="input" minlength="8"
required /></div>
<button type="submit" class="btn btn
primary" style="width:100%; padding:11px;">إ
 
 
 
 
لك 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 ✨</button>
</form>
</div>
</div>
  <!-- MAIN APP -->
<div id="appShell" class="app-shell hidden">
<div id="sidebarOverlay" class="sidebar
overlay" onclick="toggleSidebar(false)"></div>
<aside id="sidebar" class="sidebar">
<div class="brand-logo" style="padding:0
6px;">
<svg width="38" height="38" viewBox="0
0 48 48" fill="none">
<rect width="48" height="48" rx="12"
fill="url(#leevG2)"/>
<path d="M15 32C15 21 23 14 34 14C34
25 27 32 15 32Z" fill="white"/>
<defs><linearGradient id="leevG2"
x1="0" y1="0" x2="48" y2="48"><stop stopcolor="#4F46E5"/><stop offset="1" stop
color="#06B6D4"/></linearGradient></defs>
</svg>
<div><div style="font
size:1.05rem;">م
 ف</div><span
 
 
 
 
 
class="domain-pill">LEEV.study</span></div>
</div>
  <div class="nav-list">
<div class="nav-item active" data
tab="home" onclick="switchTab('home')">🏠
ت</div>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div class="nav-item" data-tab="social"
onclick="switchTab('social')">🎙️ ا
 
 
 
 
 
 
 
 
 
 
 
 
 
ت</div>
 
 
 
 
 
<div class="nav-item" data-tab="books"
onclick="switchTab('books')">📚 ا
 ت<
 
 
 
 
 
 
 
 
 
 
 
div>
<div class="nav-item" data-tab="study"
onclick="switchTab('study')">⏱️ ا
 
 
 
 
 
 
 
ر</div>
 
 
 
 
 
<div class="nav-item" data
tab="whiteboard"
onclick="switchTab('whiteboard')">🎨 ا
 
 
 
 
 
 
ة</div>
 
 
 
 
 
 
 
 
<div class="nav-item" data-tab="ai"
onclick="switchTab('ai')">🤖 ك
 
 
 
 
 
 
 
 
عي</div>
 
 
 
 
 
 
 
<div class="nav-item" data
tab="friends" onclick="switchTab('friends')">👥
اء</div> 
 
 
 
 
 
<div class="nav-item" data
tab="profile" onclick="switchTab('profile')">👤
ي</div>
 
 
 
 
 
 
 
 
 
 
<div class="nav-item hidden" data
tab="host" id="navHostItem"
onclick="switchTab('host')">👑 ل
 ك 
 
 
 
 
 
 
 
 <span
class="nav-badge hidden" id="pendingBadge">0<
span></div>
</div>
  <div style="border-top:1px solid var(-
card-border); padding-top:12px;">
<div style="display:flex; align
items:center; gap:10px; margin-bottom:10px;">
<div id="sideAvatar"
style="width:38px; height:38px; border
radius:10px; background:var(--accent);
color:#fff; display:grid; place-items:center;"
🎓</div>
<div style="min-width:0;"><div
id="sideUserName" style="font-weight:800; font
size:0.9rem;">-</div><div id="sideUserRole"
style="font-size:0.74rem; color:var(-
accent);">-</div></div>
</div>
<button class="btn btn-ghost btn-sm"
style="width:100%;" onclick="logout()">🚪 خ
 ج<
 
button>
</div>
</aside>
<main class="main-area">
<header class="topbar">
<div style="display:flex; align
items:center; gap:10px;">
<button class="btn btn-ghost btn-sm
mobile-menu-btn"
onclick="toggleSidebar(true)">☰</button>
<h2 id="pageTitle" style="font
size:1.15rem; font-weight:800;">ا
 
 
 
 
 
 
 
ت</h2>
 
 
 
 
 
 
 
 
</div>
<div style="display:flex; align
items:center; gap:8px; flex-wrap:wrap;">
<select id="topThemeSelect"
class="select" style="width:auto; padding:5px
10px; font-size:0.82rem;"
onchange="setThemePreference(this.value)">
<option value="auto">🌓 ح
 ز<
 
 
 
 
 
 
 
option>
<option value="light">☀️ ف
 ح<
 
option>
<option value="dark">🌙 غ
 ق<
 
option>
</select>
<button class="btn btn-ghost btn-sm"
onclick="enableBrowserNotifications()">🔔
ة</button>
 
 
 
 
 
 
 
 
 
 
 
 
 
<span class="pill pill-warning"
id="streakPill">🔥 1</span>
 س
 
 
 
 
 
<span class="pill pill-accent"
id="topXpPill">⚡ 0 XP</span><span class="pill pill-danger hidden"
id="demoModeBadge">👁️ م
 ط</span>
 
 
 
 
 
 
 
</div>
</header>
  <div class="content-area">
<!-- HOME -->
<section id="tab-home" class="tab
section">
<div class="glass-card"
style="margin-bottom:18px; border-right:4px
solid var(--accent); display:flex; justify
content:space-between; align-items:center;
flex-wrap:wrap; gap:10px;">
<div>
<div style="font-size:0.8rem;
color:var(--accent); font-weight:800;">📢 ت
 
 
 
 
ع:</div>
 
 
 
 
 
 
 
 
 
 
 
<div id="globalAnnouncementText"
style="font-weight:700;">-</div>
</div>
<span class="pill pill-success">🔔
100%</span>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
</div>
  <div class="glass-card hidden"
id="teacherQuickHwBox" style="margin
bottom:18px;">
<h3 style="margin-bottom:12px;">📝
اب</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<form onsubmit="addHomework(event)"class="grid-3">
<input type="text" id="hwSubject"
class="input" placeholder="ا
 ة" 
 
 
 required />
<input type="text" id="hwPage"
class="input" placeholder="ا
 ن"
 
 
 
 
 
 
 
 
 
 
 
 
 
required />
<input type="text" id="hwDue"
class="input" placeholder="ا
 ة"
 
 
 
 
 
 
 
 
 
 
required />
<input type="text" id="hwNotes"
class="input" placeholder="ت
 ة..."
 
 
 
 
 
 
 
 
 
 
style="grid-column:span 2;" />
<button type="submit" class="btn
btn-primary">+ ن
 ر</button>
 
 
 
 
 
 
 
 
 
 
 
 
 
</form>
</div>
  <div class="grid-2">
<div class="glass-card">
<h3 style="margin-bottom:12px;">📌
ت</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
<div id="homeworkList"
style="display:flex; flex-direction:column;
gap:10px;"></div>
</div>
<div class="glass-card">
<h3 style="margin-bottom:12px;">🔗
ة</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div id="interactiveLinksList"
style="display:flex; flex-direction:column;
gap:10px;"></div></div>
</div>
</section>
  <!-- SOCIAL: DISCORD + WHATSAPP +
SNAPCHAT + VOICE + SCREEN SHARE -->
<section id="tab-social" class="tab
section hidden">
<div class="glass-card"
style="margin-bottom:16px; padding:14px;">
<div style="display:flex; justify
content:space-between; align-items:center;
margin-bottom:10px;">
<strong>👻 ق
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
)</strong>
 
 
 
 
 24
 
 
 
 
<button class="btn btn-primary
btn-sm" onclick="addStorySnap()">+ أ
 
 
 
 
 
 
 
ة 
 
 
 
 
 📸</button>
</div>
<div class="stories-row"
id="storiesRow"></div>
</div>
  <div class="social-layout">
<div class="glass-card"
style="display:flex; flex-direction:column;
gap:10px;">
<div style="font-size:0.8rem;
font-weight:800; color:var(--text-muted);">💬
ة</div>
 
 
 
 
 
 
 
 
 
<button class="channel-btnactive" onclick="selectRoom('public-general',
'💬 # ا
 ة', 
 
 
 
 
 
 
 
 
 
 
 this)"><span># ا
 
 
 
 
 
 
ة</span><span 
 
 
 
 
 class="pill pill
success">ع
 م</span></button>
<button class="channel-btn"
onclick="selectRoom('public-homework', '📘 #ت', 
 
 
 
 
 
 
 
 
 
 
 this)"><span># ن
 ت<
 
 
 
 
 
 
 
 
 
 
 
span></button>
  <div style="font-size:0.8rem;
font-weight:800; color:var(--text-muted);
margin-top:6px;">🔒 ر
 ة</div>
 
 
 
 
 
 
 
<div style="display:flex;
gap:6px;">
<input type="text"
id="privateRoomCode" class="input"
style="padding:6px 10px; font-size:0.82rem;"
placeholder="ك
 ص..." 
 
 
 
 
 
 
 
 />
<button class="btn btn-primary
btn-sm" onclick="joinPrivateRoom()">د
 ل<
 
button>
</div>
  <div style="font-size:0.8rem;
font-weight:800; color:var(--text-muted);
margin-top:6px;">🔊 ا
 ة</div>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div style="padding:12px; border
radius:12px; background:rgba(16,185,129,0.1);
border:1px solid rgba(16,185,129,0.3);">
<div style="font-weight:800;
font-size:0.86rem; margin-bottom:8px;"id="voiceStatusText">🔊 غ
 ل</div>
 
 
 
 
 
<div style="display:flex;
gap:6px; flex-wrap:wrap;">
<button class="btn btn
success btn-sm" id="voiceJoinBtn"
onclick="toggleVoiceRoom()">🎙️ د
 ي<
 
 
 
 
 
 
button>
<button class="btn btn-ghost
btn-sm hidden" id="micMuteBtn"
onclick="toggleMicMute()">🎤 ك
 م</button>
<button class="btn btn
primary btn-sm" onclick="startScreenShare()">🖥️
ة</button>
 
 
 
 
 
 
 
</div>
</div>
</div>
  <div class="glass-card"
style="display:flex; flex-direction:column;
gap:10px;">
<div style="display:flex;
justify-content:space-between; align
items:center; flex-wrap:wrap; gap:8px;">
<h3 id="activeRoomTitle">💬 #
ة</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
<label style="font
size:0.82rem; display:flex; align-items:center;
gap:5px; cursor:pointer;">
<input type="checkbox"
id="snapDisappearToggle" /> 👻 ر
 
 
 
 
 
 
 
 
 
 

 
 
 
 
 
</label>
</div>
  <div id="screenShareContainer"
class="hidden" style="background:#0f172a;
border-radius:14px; padding:10px; border:2px
solid var(--accent);">
<div style="display:flex;
justify-content:space-between; align
items:center; color:#fff; margin-bottom:8px;">
<span>🖥️ ب
 ر<
 
 
 
 
 
 
 
 
 
 
 
 
 
 
span>
<button class="btn btn-danger
btn-sm" onclick="stopScreenShare()">⏹ إ
 ف<
 
 
button>
</div>
<video id="screenShareVideo"
autoplay playsinline muted style="width:100%;
max-height:260px; border-radius:10px;
background:#000;"></video>
</div>
  <div class="chat-messages"
id="socialChatBox"></div>
  <div style="display:flex;
gap:6px; flex-wrap:wrap;">
<button class="btn btn-ghost
btn-sm" onclick="sendSticker('🔥 ك
 و!')">🔥
و</button>
 
 
<button class="btn btn-ghostbtn-sm" onclick="sendSticker('📚 و
 
 
ة')">📚 
 
 
 
 
 
 
 م
 ة</button>
 
 
 
<button class="btn btn-ghost
btn-sm" onclick="sendSticker('😂')">😂</button>
<button class="btn btn-ghost
btn-sm" onclick="sendSticker('❤️')">❤️</button>
</div>
  <form
onsubmit="sendSocialMessage(event)"
style="display:flex; gap:8px; flex-wrap:wrap;">
<input type="file"
id="chatImgInput" accept="image/*"
class="hidden" onchange="sendChatImage(event)"
/>
<button type="button"
class="btn btn-ghost btn-sm"
onclick="document.getElementById('chatImgInput
).click()">🖼️ ص
 ة</button>
 
<button type="button"
class="btn btn-ghost btn-sm" id="voiceNoteBtn"
onclick="toggleVoiceNote()">🎙️ ص
 ت</button>
<input type="text"
id="socialMsgInput" class="input"
style="flex:1; min-width:160px;"
placeholder="ا
 ك..." 
 
 
 
 
 
 
 
 />
<button type="submit"
class="btn btn-primary">إ
 ل 
 
 ➤</button>
</form>
</div>
</div></section>
  <!-- BOOKS -->
<section id="tab-books" class="tab
section hidden">
<div class="glass-card"
style="margin-bottom:16px; display:flex;
justify-content:space-between; align
items:center; flex-wrap:wrap; gap:10px;">
<h3>📚 ا
 ت</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<input type="text" id="bookSearch"
class="input" style="max-width:260px;"
placeholder="🔍 ا
 ب..."
 
 
 
 
 
 
 
 
 
oninput="renderBooks()" />
</div>
<div id="booksGrid" class="grid-3"><
div>
</section>
  <!-- STUDY -->
<section id="tab-study" class="tab
section hidden">
<div class="grid-2">
<div class="glass-card"
style="text-align:center;">
<h3>⏱️ ب
 ر</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div class="pomodoro-circle"
id="pomodoroRing"><div class="pomodoro
inner"><div class="pomodoro-time"
id="pomodoroDisplay">25:00</div><div
style="font-size:0.84rem; color:var(-accent);">ت
 ز</div></div></div>
 
 
<div style="display:flex;
justify-content:center; gap:8px; margin
bottom:14px;">
<button class="btn btn-primary"
id="pomoToggleBtn" onclick="togglePomodoro()">▶أ</button>
 
 
 
<button class="btn btn-ghost"
onclick="resetPomodoro()">↺ إ
 ة</button>
 
 
</div>
<div style="display:flex;
justify-content:center; gap:8px; flex
wrap:wrap;">
<button class="btn btn-ghost
btn-sm" onclick="toggleAmbientSound('rain')">🌧️
ئ</button>
 
 
 
 
 
 
 
<button class="btn btn-ghost
btn-sm" onclick="toggleAmbientSound('heavy')">⛈️
ر</button>
 
 
 
 
 
 
 
<button class="btn btn-danger
btn-sm" onclick="stopAmbientSound()">⏹ إ
 ف<
 
 
button>
</div>
</div>
<div class="glass-card"
style="display:flex; flex-direction:column;">
<div style="display:flex;
justify-content:space-between; margin
bottom:10px;">
<h3>📓 م
 ب</h3>
 
 
 
 
 
 
 
 
 
<button class="btn btn-successbtn-sm" onclick="saveStudentNotes()">💾 ح
 ظ<
button>
</div>
<textarea id="studentNotesArea"
class="textarea" style="flex:1; min
height:280px;" placeholder="ا
 تك..."><
 
 
 
 
 
 
 
 
 
textarea>
</div>
</div>
</section>
  <!-- WHITEBOARD -->
<section id="tab-whiteboard"
class="tab-section hidden">
<div class="glass-card">
<div style="display:flex; justify
content:space-between; margin-bottom:10px;">
<h3>🎨 ا
 ة</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div style="display:flex;
gap:6px;">
<button class="btn btn-primary
btn-sm" onclick="downloadWhiteboard()">⬇ ح
 
 
ة</button>
 
 
 
<button class="btn btn-danger
btn-sm" onclick="clearWhiteboard()">🗑️ م
 ح<
button>
</div>
</div>
<canvas id="whiteboardCanvas"><
canvas>
</div></section>
  <!-- ALL AI -->
<section id="tab-ai" class="tab-section
hidden">
<div id="aiDisabledNotice"
class="glass-card hidden" style="text
align:center; padding:30px;"><h3>🔒 ا
 
 
 
 
 
 
ك</h3></div>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div id="aiEnabledContent">
<div class="grid-4" style="margin
bottom:18px;">
<a href="https:/
gemini.google.com/" target="_blank"
rel="noopener noreferrer" class="glass-card"
style="text-decoration:none; color:inherit;">✨
<b>Google Gemini</b></a>
<a href="https://grok.com/"
target="_blank" rel="noopener noreferrer"
class="glass-card" style="text-decoration:none;
color:inherit;">⚡ <b>Grok AI</b></a>
<a href="https://chatgpt.com/"
target="_blank" rel="noopener noreferrer"
class="glass-card" style="text-decoration:none;
color:inherit;">🧠 <b>ChatGPT</b></a>
<a href="https://claude.ai/"
target="_blank" rel="noopener noreferrer"
class="glass-card" style="text-decoration:none;
color:inherit;">🌟 <b>Claude AI</b></a>
<a href="https:/
notebooklm.google.com/" target="_blank"rel="noopener noreferrer" class="glass-card"
style="text-decoration:none; color:inherit;">📓
<b>NotebookLM</b></a>
<a href="https:/
www.perplexity.ai/" target="_blank"
rel="noopener noreferrer" class="glass-card"
style="text-decoration:none; color:inherit;">🔍
<b>Perplexity</b></a>
<a href="https://docs.google.com
document/u/0/" target="_blank" rel="noopener
noreferrer" class="glass-card" style="text
decoration:none; color:inherit;">📄 <b>Google
Docs</b></a>
</div>
<div class="glass-card">
<h3 style="margin-bottom:10px;">🤖
ج</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div class="chat-messages"
id="aiChatBox" style="height:240px; margin
bottom:10px;"></div>
<form
onsubmit="sendAiMessage(event)"
style="display:flex; gap:8px;">
<input type="text" id="aiInput"
class="input" placeholder="ا
 
 
 
 
 
 
 
 
 
 
 
ي..." 
 
 
 
 required />
<button type="submit"
class="btn btn-primary">إ
 ل</button>
 
 
</form>
</div>
</div></section>
  <!-- FRIENDS -->
<section id="tab-friends" class="tab
section hidden">
<div class="glass-card" style="max
width:600px;">
<h3 style="margin-bottom:12px;">👥
م</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<form
onsubmit="addFriendByUsername(event)"
style="display:flex; gap:8px; margin
bottom:14px;">
<input type="text"
id="friendSearchInput" class="input"
placeholder="ي
 ك" 
 
 
 
 
 
 
 
 
 
 dir="ltr" required 
>
<button type="submit" class="btn
btn-primary">+ إ
 ة</button>
 
 
</form>
<div id="myFriendsList"
style="display:flex; flex-direction:column;
gap:8px;"></div>
</div>
</section>
  <!-- PROFILE -->
<section id="tab-profile" class="tab
section hidden">
<div class="glass-card" style="max
width:600px;"><h3 style="margin-bottom:14px;">👤
ي</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<form
onsubmit="saveProfile(event)">
<div class="form-group"><label
class="form-label">ا
 سم</label><input
 
type="text" id="profName" class="input"
required /></div>
<div class="form-group"><label
class="form-label">ا
 ة</label><select
 
 
 
 
 
 
 
 
 
 
 
id="profAvatar" class="select"><option
value="🎓">🎓</option><option value="🚀">🚀<
option><option value="🦁">🦁</option><option
value="⚡">⚡</option><option value="🦉">🦉<
option></select></div>
<div class="form-group"><label
class="form-label">ا
 ة</label><input
 
 
 
type="text" id="profBio" class="input" /></div>
<button type="submit" class="btn
btn-primary">💾 ح
 ظ</button>
</form>
</div>
</section>
  <!-- OWNER PANEL -->
<section id="tab-host" class="tab
section hidden">
<div class="grid-2" style="margin
bottom:16px;">
<div class="glass-card">
<h3 style="margin-bottom:12px;">⚙️ة</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
<div style="display:flex;
gap:8px; margin-bottom:12px;">
<input type="text"
id="hostAnnouncementInput" class="input"
placeholder="ت
 ن..." ل
 
 
 
 
 
 
 
 
 />
<button class="btn btn-primary
btn-sm" onclick="updateAnnouncement()">ت
 ث<
 
 
button>
</div>
<div style="display:flex;
justify-content:space-between; align
items:center;">
<strong>🤖 ت
 
 
 
 
 
 
 
 
 
 
 
عي</strong>
 
 
 
 
 
 
 
<button class="btn btn-sm btn
success" id="aiToggleBtn"
onclick="toggleGlobalAI()">م
 ل 
 ✅</button>
</div>
</div>
<div class="glass-card">
<h3 style="margin-bottom:12px;">📚
ط</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<form onsubmit="addBook(event)"
style="display:flex; gap:6px; flex-wrap:wrap;
margin-bottom:10px;">
<input type="text"
id="bookTitle" class="input" style="flex:1;"
placeholder="ا
 ب" 
 
 
 
 
 
 
 required />
<input type="text"
id="bookSubject" class="input" style="flex:1;"placeholder="ا
 ة" 
 
 
 required />
<input type="url" id="bookUrl"
class="input" placeholder="https://..."
dir="ltr" required />
<button type="submit"
class="btn btn-primary btn-sm">+ ك
 ب</button>
 
</form>
<form onsubmit="addLink(event)"
style="display:flex; gap:6px; flex-wrap:wrap;">
<input type="text"
id="linkTitle" class="input" style="flex:1;"
placeholder="ع
 ط" 
 
 
 
 
 
 
 
 
 required />
<input type="url" id="linkUrl"
class="input" style="flex:1;"
placeholder="https://..." dir="ltr" required />
<button type="submit"
class="btn btn-success btn-sm">+ ر
 ط</button>
 
</form>
</div>
</div>
  <div class="grid-2" style="margin
bottom:16px;">
<div class="glass-card"><h3
style="margin-bottom:10px;">⏳ ط
 
 
 
 
 
 
 
 
 
 
 
دة</h3><div 
 
 
 
 
 id="pendingStudentsList"
style="display:flex; flex-direction:column;
gap:8px;"></div></div>
<div class="glass-card"><h3
style="margin-bottom:10px;">👥 ا
 
 
 
 
 
 
ت</h3><div ي
 
 
 
 
 
 
 
 id="approvedUsersList"style="display:flex; flex-direction:column;
gap:8px;"></div></div>
</div>
  <div class="glass-card"
style="border-color:var(--danger);">
<h3 style="margin-bottom:10px;">🛡️
ه</h3>
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
<div id="securityLogsBox"
style="display:flex; flex-direction:column;
gap:6px; max-height:200px; overflow-y:auto;"><
div>
</div>
</section>
</div>
</main>
</div>
  <script src="/socket.io/socket.io.js"><
script>
<script>
function el(tag, attrs, text) {
var n = document.createElement(tag);
attrs = attrs || {};
Object.keys(attrs).forEach(function(k) {
if (k === 'className') n.className =
attrs[k];
else if (k === 'style') n.style.cssText
= attrs[k];
else if (k.indexOf('on') === 0 &&
typeof attrs[k] === 'function')n.addEventListener(k.slice(2), attrs[k]);
else n.setAttribute(k, attrs[k]);
});
if (text) n.textContent = text;
return n;
}
function safeUrl(u) { try { var x = new
URL(u); return (x.protocol === 'https:' ||
x.protocol === 'http:') ? x.href : '#'; } catch
(e) { return '#'; } }
function showToast(m) { var t = el('div', {
className: 'toast' }, m);
document.body.appendChild(t);
setTimeout(function() { t.remove(); }, 3000); }
  // Theme Mode (Auto / Light / Dark)
var mediaDark =
window.matchMedia('(prefers-color-scheme:
dark)');
function applyThemeMode(pref) {
var mode = pref === 'auto' ?
(mediaDark.matches ? 'dark' : 'light') : pref;
document.body.setAttribute('data-mode',
mode);
document.getElementById('authThemeSelect
).value = pref;
document.getElementById('topThemeSelect'
.value = pref;
}
function setThemePreference(pref) {
localStorage.setItem('leev_theme_pref', pref);applyThemeMode(pref); }
mediaDark.addEventListener('change',
function() { if
((localStorage.getItem('leev_theme_pref') ||
'auto') === 'auto') applyThemeMode('auto'); });
applyThemeMode(localStorage.getItem('leev_
heme_pref') || 'auto');
  var csrfToken = '';
var currentUser = null;
var appState = { settings: {}, homework:
[], books: [], links: [], stories: [],
friendsList: [] };
var activeRoom = 'public-general';
var roomMessages = {};
var socket = null;
  function refreshCsrf() {
return fetch('/api/csrf-token', {
credentials: 'include' })
.then(function(r) { return r.json(); })
.then(function(d) { if (d &&
d.csrfToken) csrfToken = d.csrfToken; })
.catch(function() {});
}
refreshCsrf();
  function api(url, method, body) {
var opts = { method: method || 'GET',
credentials: 'include', headers: { 'Content
Type': 'application/json', 'X-CSRF-Token':csrfToken } };
if (body) opts.body =
JSON.stringify(body);
return fetch(url, opts).then(function(r)
{ return r.json().then(function(d) { if (!r.ok)
throw new Error(d.error || 'خ
 أ'); return d;
}); });
}
  function switchAuthTab(tab) {
document.getElementById('loginForm').cla
sList.toggle('hidden', tab !== 'login');
document.getElementById('registerForm').
lassList.toggle('hidden', tab === 'login');
document.getElementById('tabLoginBtn').c
assName = tab === 'login' ? 'btn btn-primary' :
'btn btn-ghost';
document.getElementById('tabRegBtn').cla
sName = tab !== 'login' ? 'btn btn-primary' :
'btn btn-ghost';
}
  function handleLogin(e) {
e.preventDefault();
api('/api/auth/login', 'POST', {
identifier:
document.getElementById('loginIdentifier').val
e,
password:
document.getElementById('loginPassword').value
}).then(function(d) {currentUser = d.user;
bootApp();
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
  function handleRegister(e) {
e.preventDefault();
api('/api/auth/register', 'POST', {
name:
document.getElementById('regName').value,
username:
document.getElementById('regUsername').value,
email:
document.getElementById('regEmail').value,
password:
document.getElementById('regPassword').value
}).then(function(d) {
e.target.reset();
switchAuthTab('login');
showToast('✅ ' + d.message);
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
  function enterDemoStudent() {
api('/api/auth/demo', 'POST',
{}).then(function(d) {
currentUser = d.user;
bootApp();
showToast('👁️ د
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
)');
 
 
 
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
  function logout() {
api('/api/auth/logout', 'POST',
{}).catch(function() {}).finally(function() {
currentUser = null;
if (socket) { socket.disconnect();
socket = null; }
stopVoiceRoom(); stopScreenShare();
stopAmbientSound();
document.getElementById('appShell').cl
ssList.add('hidden');
document.getElementById('authScreen').
lassList.remove('hidden');
});
}
  function bootApp() {
document.getElementById('authScreen').cl
ssList.add('hidden');
document.getElementById('appShell').clas
List.remove('hidden');
loadBootstrap();
connectSocket();
initWhiteboard();
}
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
api('/api/bootstrap',
'GET').then(function(d) {
currentUser = d.user;
appState = d;
document.getElementById('authScreen').cl
ssList.add('hidden');
document.getElementById('appShell').clas
List.remove('hidden');
renderAll();
connectSocket();
initWhiteboard();
}).catch(function() {});
  function loadBootstrap() {
api('/api/bootstrap',
'GET').then(function(d) {
currentUser = d.user;
appState = d;
renderAll();
});
}
  function connectSocket() {
if (socket) socket.disconnect();
socket = io({ withCredentials: true });
socket.on('connect', function() {
socket.emit('join-room', activeRoom); });
socket.on('room-history', function(d) {
roomMessages[d.roomId] = d.messages || []; if
(d.roomId === activeRoom) renderSocialChat();
});socket.on('chat-message', function(d) {
if (!roomMessages[d.roomId])
roomMessages[d.roomId] = [];
roomMessages[d.roomId].push(d.msg);
if (d.roomId === activeRoom)
renderSocialChat();
});
socket.on('delete-message', function(d) {
roomMessages[d.roomId] =
(roomMessages[d.roomId] ||
[]).filter(function(m) { return m.id !==
d.msgId; });
if (d.roomId === activeRoom)
renderSocialChat();
});
socket.on('new-homework', function(hw) {
appState.homework.unshift(hw);
renderHomework();
showToast('🔔 و
 د: 
 
 
 
 
 
 ' +
hw.subject);
if ('Notification' in window &&
Notification.permission === 'granted') {
new Notification('و
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
ف!', 
 
 { body: hw.subject + ': ' + hw.page });
}
});
socket.on('settings-updated', function(s)
{ appState.settings = s; renderAll(); });
}
  function isDemoLocked() {if (currentUser && currentUser.role ===
'demo') { showToast('🔒 و
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
ط.'); 
 
 
 
 
 
 
 
 
 
 
 return true; }
return false;
}
  function renderAll() {
if (!currentUser) return;
var isOwner = currentUser.role ===
'owner';
var isTeacher = isOwner ||
currentUser.role === 'teacher';
document.getElementById('demoModeBadge')
classList.toggle('hidden', currentUser.role !==
'demo');
document.getElementById('navHostItem').c
assList.toggle('hidden', !isOwner);
document.getElementById('teacherQuickHwB
x').classList.toggle('hidden', !isTeacher);
document.getElementById('sideUserName').
extContent = currentUser.name;
document.getElementById('sideAvatar').te
tContent = currentUser.avatar || '🎓';
var roles = { owner: '👑 ا
 ي',
 
 
 
 
 
 
 
 
 
 
teacher: '👨‍🏫 م
 م', 
 student: '🎓 ط
 ب', 
 demo: '👁️
ط' 
 
 
 
 
 
 
 };
 
document.getElementById('sideUserRole').
extContent = roles[currentUser.role] || 'ط
 ب';
 
document.getElementById('topXpPill').tex
Content = '⚡ ' + (currentUser.xp || 0) + ' XP';
document.getElementById('streakPill').tetContent = '🔥 س
 ك: 
 
 ' + (currentUser.streak ||
1);
document.getElementById('globalAnnouncem
ntText').textContent =
appState.settings.announcement || '';
document.getElementById('studentNotesAre
').value = currentUser.notes || '';
document.getElementById('profName').value
= currentUser.name || '';
document.getElementById('profBio').value
= currentUser.bio || '';
document.getElementById('profAvatar').va
ue = currentUser.avatar || '🎓';
  var aiOn = appState.settings.aiEnabled 
== false;
document.getElementById('aiEnabledConten
').classList.toggle('hidden', !aiOn);
document.getElementById('aiDisabledNotic
').classList.toggle('hidden', aiOn);
  renderHomework();
renderLinks();
renderBooks();
renderStories();
renderFriends();
if (isOwner) loadAdminPanel();
}
  function switchTab(id) {
if (id === 'host' && (!currentUser ||currentUser.role !== 'owner')) { showToast('🚨
ع!'); 
 
 
 return; }
 
document.querySelectorAll('.tab
section').forEach(function(s) {
s.classList.add('hidden'); });
document.getElementById('tab-' +
id).classList.remove('hidden');
document.querySelectorAll('.nav
item').forEach(function(n) {
n.classList.remove('active'); });
var active =
document.querySelector('.nav-item[data-tab="' +
id + '"]');
if (active)
active.classList.add('active');
toggleSidebar(false);
if (id === 'whiteboard') resizeCanvas();
if (id === 'host') loadAdminPanel();
}
function toggleSidebar(o) {
document.getElementById('sidebar').classList.t
ggle('open', o);
document.getElementById('sidebarOverlay').clas
List.toggle('open', o); }
  function enableBrowserNotifications() {
if (!('Notification' in window)) return
showToast('ا
 ت');
 ر
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
Notification.requestPermission().then(fu
ction(p) { if (p === 'granted') showToast('🔔 ت
 
اً!'); 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });}
  function addHomework(e) {
e.preventDefault();
api('/api/homework', 'POST', {
subject:
document.getElementById('hwSubject').value,
page:
document.getElementById('hwPage').value,
due:
document.getElementById('hwDue').value,
notes:
document.getElementById('hwNotes').value
}).then(function(d) {
appState.homework = d.homework;
e.target.reset(); renderHomework();
showToast('✅ ت
 ب');
 
 
 
 
 
 
 
 
 
 
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
  function renderHomework() {
var box =
document.getElementById('homeworkList');
box.innerHTML = '';
var isAdmin = currentUser.role ===
'owner' || currentUser.role === 'teacher';
appState.homework.forEach(function(h) {
var done = (h.completedBy ||
[]).indexOf(currentUser.id) !== -1;
var card = el('div', { style:'padding:12px; border-radius:12px;
background:var(--input-bg); border:1px solid '
+ (done ? 'var(--success)' : 'var(--card
border)') });
card.appendChild(el('span', {
className: 'pill pill-accent' }, '📘 ' +
h.subject));
card.appendChild(el('div', { style:
'font-weight:800; margin:6px 0;' }, h.page));
card.appendChild(el('div', { style:
'font-size:0.8rem; color:var(--text-muted);' },
'⏰ ' + h.due + (h.notes ? ' • ' + h.notes :
'')));
var row = el('div', { style:
'display:flex; gap:8px; margin-top:8px;' });
row.appendChild(el('button', {
className: 'btn btn-sm ' + (done ? 'btn
success' : 'btn-ghost'), onclick: function() {
if (isDemoLocked()) return;
api('/api/homework/' + h.id + '
toggle', 'POST', {}).then(function(d) {
appState.homework = d.homework; currentUser.xp
= d.xp; renderAll(); });
} }, done ? '✅ م
 ز 
 
 
 
 
 
 
 
 
 
 
 
 
 (+15
XP)'));
if (isAdmin) {
row.appendChild(el('button', {
className: 'btn btn-sm btn-danger', onclick:
function() {
api('/api/homework/' + h.id,
'DELETE').then(function(d) { appState.homework= d.homework; renderHomework(); 
 
 
 
 
 
 
 
 }); 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
}
card.appendChild(row);
box.appendChild(card);
});
}
  function renderLinks() {
var box =
document.getElementById('interactiveLinksList'
; box.innerHTML = '';
appState.links.forEach(function(l) {
var r = el('div', { style:
'display:flex; justify-content:space-between;
align-items:center; padding:10px; border
radius:12px; background:var(--input-bg);' });
r.appendChild(el('span', { style:
'font-weight:700;' }, '🔗 ' + l.title));
r.appendChild(el('a', { href:
safeUrl(l.url), target: '_blank', rel:
'noopener noreferrer', className: 'btn btn
primary btn-sm' }, 'ف
 ح ↗'));
box.appendChild(r);
});
}
  function renderBooks() {
var q =
(document.getElementById('bookSearch').value ||
'').toLowerCase();var grid =
document.getElementById('booksGrid');
grid.innerHTML = '';
appState.books.filter(function(b) {
return b.title.toLowerCase().indexOf(q) !== -1;
}).forEach(function(b) {
var c = el('div', { className: 'glass
card' });
c.appendChild(el('span', { className:
'pill pill-accent' }, b.subject));
c.appendChild(el('h4', { style:
'margin:10px 0;' }, b.title));
c.appendChild(el('a', { href:
safeUrl(b.url), target: '_blank', rel:
'noopener noreferrer', className: 'btn btn
primary btn-sm' }, 'ف
 ب 
 
 
 
 
 
 
 ↗'));
grid.appendChild(c);
});
}
  // Social Hub
function renderStories() {
var row =
document.getElementById('storiesRow');
row.innerHTML = '';
(appState.stories ||
[]).forEach(function(s) {
var item = el('div', { className:
'story-circle', onclick: function() {
showToast('👻 ' + s.user + ': ' + s.text); } });
var ring = el('div', { className:'story-ring' });
ring.appendChild(el('div', { className:
'story-inner' }, s.avatar || '🎓'));
item.appendChild(ring);
item.appendChild(el('div', { style:
'font-size:0.75rem; font-weight:700;' },
s.user));
row.appendChild(item);
});
}
function addStorySnap() {
if (isDemoLocked()) return;
var txt = prompt('ا
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
م:');
 
 
 
 
if (!txt) return;
api('/api/stories', 'POST', { text: txt
}).then(function(d) {
appState.stories = d.stories;
currentUser.streak = d.streak; currentUser.xp =
d.xp; renderAll(); showToast('🔥 ت
 
 
 
 
 
 
 
 
 
 
 
 
 
 
ك!');
 
 
 
 
 
 
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
function selectRoom(id, title, btn) {
activeRoom = id;
document.getElementById('activeRoomTitle
).textContent = title;
document.querySelectorAll('.channel
btn').forEach(function(b) {
b.classList.remove('active'); });if (btn) btn.classList.add('active');
if (socket) socket.emit('join-room', id);
renderSocialChat();
}
function joinPrivateRoom() {
var code =
document.getElementById('privateRoomCode').val
e.trim().replace(/[^a-zA-Z0-9_-]/g, '');
if (!code) return showToast('ا
 
 
 
 
 
 
 
ص');
 
 
 
 
 
 
 
 
 
 
selectRoom('priv-' + code, '🔒 ر
 ص: 
 
 
 
 '
+ code, null);
}
function renderSocialChat() {
var box =
document.getElementById('socialChatBox');
box.innerHTML = '';
(roomMessages[activeRoom] ||
[]).forEach(function(m) {
var mine = m.sender === (currentUser &&
currentUser.name);
var b = el('div', { className: 'msg
bubble ' + (mine ? 'msg-mine' : 'msg-other')
});
b.appendChild(el('div', { style: 'font
size:0.74rem; font-weight:800; opacity:0.85;'
}, m.sender + (m.disappear ? ' 👻' : '')));
if (m.type === 'image' && m.data)
b.appendChild(el('img', { src: m.data, style:
'max-width:210px; border-radius:10px; margin
top:6px;' }));else if (m.type === 'audio' && m.data)
b.appendChild(el('audio', { controls: 'true',
src: m.data, style: 'max-width:210px; margin
top:6px;' }));
else b.appendChild(el('div', {},
m.content));
b.appendChild(el('div', { style: 'font
size:0.68rem; opacity:0.75; margin-top:4px;
text-align:left;' }, m.time + ' ✓✓'));
box.appendChild(b);
});
box.scrollTop = box.scrollHeight;
}
function sendSocialMessage(e) {
e.preventDefault(); if (isDemoLocked())
return;
var inp =
document.getElementById('socialMsgInput');
if (!inp.value.trim()) return;
socket.emit('chat-message', { roomId:
activeRoom, type: 'text', content:
inp.value.trim(), disappear:
document.getElementById('snapDisappearToggle')
checked });
inp.value = '';
}
function sendSticker(s) {
if (isDemoLocked()) return;
socket.emit('chat-message', { roomId:
activeRoom, type: 'text', content: s,
disappear:document.getElementById('snapDisappearToggle')
checked });
}
function sendChatImage(e) {
if (isDemoLocked()) return;
var f = e.target.files[0];
if (!f || f.type.indexOf('image/') !== 0
|| f.size > 1500000) return showToast('ا
 
 
 
 
 
 
 
 
1.5MB');
 
 
 
 
 
 
 
var r = new FileReader();
r.onload = function(ev) {
socket.emit('chat-message', { roomId:
activeRoom, type: 'image', content: 'ص
 ة',
 
data: ev.target.result }); };
r.readAsDataURL(f);
}
  var voiceRec = null, voiceChunks = [],
voiceStream = null, isMicMuted = false,
screenStream = null;
function toggleVoiceNote() {
if (isDemoLocked()) return;
var btn =
document.getElementById('voiceNoteBtn');
if (voiceRec && voiceRec.state ===
'recording') { voiceRec.stop(); btn.textContent
= '🎙️ ص
 ت'; btn.classList.remove('btn-danger');
return; }
navigator.mediaDevices.getUserMedia({
audio: true }).then(function(stream) {
voiceRec = new MediaRecorder(stream);voiceChunks = [];
voiceRec.ondataavailable = function(ev)
{ voiceChunks.push(ev.data); };
voiceRec.onstop = function() {
var blob = new Blob(voiceChunks, {
type: 'audio/webm' });
var r = new FileReader();
r.onload = function(ev) {
socket.emit('chat-message', { roomId:
activeRoom, type: 'audio', content: 'ر
 
 
 
 
ة', 
 
 
 
 data: ev.target.result }); };
r.readAsDataURL(blob);
stream.getTracks().forEach(function(
) { t.stop(); });
};
voiceRec.start(); btn.textContent = '⏹ل'; 
 
 
 btn.classList.add('btn-danger');
 
}).catch(function() { showToast('⚠️ ي
 
 
 
ن'); 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
function toggleVoiceRoom() {
if (voiceStream) return stopVoiceRoom();
navigator.mediaDevices.getUserMedia({
audio: true }).then(function(s) {
voiceStream = s;
document.getElementById('voiceStatusTe
t').textContent = '🟢 م
 ي';
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
document.getElementById('voiceJoinBtn'
.textContent = '📞 م
 ة';
 
 
 
document.getElementById('voiceJoinBtn'
.className = 'btn btn-danger btn-sm';document.getElementById('micMuteBtn').
lassList.remove('hidden');
}).catch(function() { showToast('⚠️ ت
 
 
 
ن'); 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 });
}
function stopVoiceRoom() {
if (voiceStream) {
voiceStream.getTracks().forEach(function(t) {
t.stop(); }); voiceStream = null; }
document.getElementById('voiceStatusText
).textContent = '🔊 غ
 ل';
 
 
 
 
 
document.getElementById('voiceJoinBtn').
extContent = '🎙️ د
 ي';
 
 
 
 
 
 
document.getElementById('voiceJoinBtn').
lassName = 'btn btn-success btn-sm';
document.getElementById('micMuteBtn').cl
ssList.add('hidden');
}
function toggleMicMute() {
if (!voiceStream) return; isMicMuted = 
isMicMuted;
voiceStream.getAudioTracks().forEach(fun
tion(t) { t.enabled = !isMicMuted; });
document.getElementById('micMuteBtn').te
tContent = isMicMuted ? '🔇 إ
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
م';
 
 
}
function startScreenShare() {
navigator.mediaDevices.getDisplayMedia({
video: true, audio: true }).then(function(s) {
screenStream = s;document.getElementById('screenShareVi
eo').srcObject = s;
document.getElementById('screenShareCo
tainer').classList.remove('hidden');
s.getVideoTracks()[0].onended =
stopScreenShare;
}).catch(function() { showToast('ت
 
 
 
 
 
 
 
ة'); 
 
 
 
 
 
 
 
 
 
 
 
 });
}
function stopScreenShare() {
if (screenStream) {
screenStream.getTracks().forEach(function(t) {
t.stop(); }); screenStream = null; }
document.getElementById('screenShareCont
iner').classList.add('hidden');
}
  // Pomodoro + Rain + Whiteboard + AI +
Friends + Profile + Admin
var pomoTotal = 1500, pomoLeft = 1500,
pomoInt = null;
function togglePomodoro() {
if (pomoInt) { clearInterval(pomoInt);
pomoInt = null;
document.getElementById('pomoToggleBtn').textC
ntent = '▶ ا
 ف'; 
 
 
 
 }
else {
document.getElementById('pomoToggleBtn
).textContent = '⏸ إ
 ف';
 
 
pomoInt = setInterval(function() {
pomoLeft--; if (pomoLeft <= 0) {clearInterval(pomoInt); pomoInt = null;
pomoLeft = 0; showToast('🎉 ا
 ة!'); 
 
 
 
 
 
 
 
 
 }
var m = String(Math.floor(pomoLeft /
60)).padStart(2, '0'), s = String(pomoLeft %
60).padStart(2, '0');
document.getElementById('pomodoroDis
lay').textContent = m + ':' + s;
document.getElementById('pomodoroRin
').style.setProperty('--progress', ((pomoLeft /
pomoTotal) * 100) + '%');
}, 1000);
}
}
function resetPomodoro() {
clearInterval(pomoInt); pomoInt = null;
pomoLeft = 1500;
document.getElementById('pomodoroDisplay').tex
Content = '25:00'; }
var aCtx = null, nNode = null;
function toggleAmbientSound(t) {
stopAmbientSound();
var AC = window.AudioContext ||
window.webkitAudioContext; aCtx = new AC();
var buf = aCtx.createBuffer(1,
aCtx.sampleRate * 3, aCtx.sampleRate), out =
buf.getChannelData(0), l = 0;
for (var i = 0; i < out.length; i++) {
var w = Math.random() * 2 - 1; out[i] = (l +
0.04 * w) / 1.04; l = out[i]; out[i] *= t ===
'heavy' ? 2.4 : 1.4; }
nNode = aCtx.createBufferSource();nNode.buffer = buf; nNode.loop = true;
var f = aCtx.createBiquadFilter(); f.type
= 'bandpass'; f.frequency.value = 950;
var g = aCtx.createGain(); g.gain.value =
0.35;
nNode.connect(f); f.connect(g);
g.connect(aCtx.destination); nNode.start();
}
function stopAmbientSound() { if (nNode) {
try { nNode.stop(); } catch (e) {} nNode =
null; } if (aCtx) { try { aCtx.close(); } catch
(e) {} aCtx = null; } }
function saveStudentNotes() {
if (isDemoLocked()) return;
api('/api/notes', 'POST', { notes:
document.getElementById('studentNotesArea').va
ue }).then(function() { showToast('💾 ت
 
 
 
 
 
تك'); 
 
 
 
 
 
 });
}
  var wbC, wbCtx, wbDr = false;
function initWhiteboard() {
wbC =
document.getElementById('whiteboardCanvas');
wbCtx = wbC.getContext('2d'); resizeCanvas();
function pos(e) { var r =
wbC.getBoundingClientRect(); return { x:
(e.touches ? e.touches[0].clientX : e.clientX)
- r.left, y: (e.touches ? e.touches[0].clientY
: e.clientY) - r.top }; }
wbC.onmousedown = wbC.ontouchstart =function(e) { wbDr = true; var p = pos(e);
wbCtx.beginPath(); wbCtx.moveTo(p.x, p.y); };
wbC.onmousemove = wbC.ontouchmove =
function(e) { if (!wbDr) return;
e.preventDefault(); var p = pos(e);
wbCtx.lineWidth = 3; wbCtx.strokeStyle =
'#4f46e5'; wbCtx.lineTo(p.x, p.y);
wbCtx.stroke(); };
wbC.onmouseup = wbC.ontouchend =
function() { wbDr = false; };
}
function resizeCanvas() { if (!wbC) return;
var w = wbC.parentElement.clientWidth - 36; if
(w > 0 && wbC.width !== w) { wbC.width = w;
wbC.height = 430; wbCtx.fillStyle = '#fff';
wbCtx.fillRect(0, 0, w, 430); } }
function clearWhiteboard() {
wbCtx.fillStyle = '#fff'; wbCtx.fillRect(0, 0,
wbC.width, wbC.height); }
function downloadWhiteboard() { var a =
document.createElement('a'); a.download =
'leev-board.png'; a.href = wbC.toDataURL();
a.click(); }
  function sendAiMessage(e) {
e.preventDefault(); if (isDemoLocked())
return;
var i =
document.getElementById('aiInput'), q =
i.value.trim(); if (!q) return;
var box =document.getElementById('aiChatBox');
box.appendChild(el('div', { className:
'msg-bubble msg-mine' }, q)); i.value = '';
api('/api/ai/ask', 'POST', { question: q
}).then(function(d) {
box.appendChild(el('div', { className:
'msg-bubble msg-other' }, d.reply));
box.scrollTop = box.scrollHeight;
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
  function addFriendByUsername(e) {
e.preventDefault(); if (isDemoLocked())
return;
api('/api/friends/add', 'POST', {
username:
document.getElementById('friendSearchInput').v
lue }).then(function(d) {
showToast('🤝 ت
 ة 
 
 
 
 
 ' +
d.targetName);
document.getElementById('friendSearchInput').v
lue = ''; loadBootstrap();
}).catch(function(err) { showToast('❌ ' +
err.message); });
}
function renderFriends() {
var box =
document.getElementById('myFriendsList');
box.innerHTML = '';
(appState.friendsList ||[]).forEach(function(f) {
box.appendChild(el('div', { style:
'padding:10px; border-radius:10px;
background:var(--input-bg);' }, (f.avatar ||
'🎓') + ' ' + f.name + ' (@' + f.username + ') •
🔥 ' + f.streak));
});
}
function saveProfile(e) {
e.preventDefault(); if (isDemoLocked())
return;
api('/api/profile', 'POST', {
name:
document.getElementById('profName').value,
avatar:
document.getElementById('profAvatar').value,
bio:
document.getElementById('profBio').value
}).then(function(d) { currentUser =
d.user; renderAll(); showToast('✅ ت
 ث');
 
 
 
 
 
 
 
});
}
  // Owner Admin Actions
function loadAdminPanel() {
api('/api/admin/panel',
'GET').then(function(d) {
var b =
document.getElementById('pendingBadge');
b.textContent = d.pending.length;
b.classList.toggle('hidden', d.pending.length=== 0);
var pBox =
document.getElementById('pendingStudentsList');
pBox.innerHTML = '';
d.pending.forEach(function(u) {
var r = el('div', { style:
'display:flex; justify-content:space-between;
align-items:center; padding:8px; border
radius:10px; background:var(--input-bg);' });
r.appendChild(el('span', {}, u.name +
' (@' + u.username + ')'));
var btns = el('div', { style:
'display:flex; gap:6px;' });
btns.appendChild(el('button', {
className: 'btn btn-success btn-sm', onclick:
function() { api('/api/admin/approve', 'POST',
{ userId: u.id }).then(loadAdminPanel); } },
'ق
 ل'));
 
btns.appendChild(el('button', {
className: 'btn btn-danger btn-sm', onclick:
function() { api('/api/admin/reject', 'POST', {
userId: u.id }).then(loadAdminPanel); } },
'ر
 ض'));
r.appendChild(btns);
pBox.appendChild(r);
});
var aBox =
document.getElementById('approvedUsersList');
aBox.innerHTML = '';
d.approved.forEach(function(u) {
var r = el('div', { style:'display:flex; justify-content:space-between;
align-items:center; padding:8px; border
radius:10px; background:var(--input-bg);' });
r.appendChild(el('span', {}, u.name +
' (' + u.role + ')'));
if (u.role !== 'owner') {
var nextRole = u.role === 'teacher'
? 'student' : 'teacher';
r.appendChild(el('button', {
className: 'btn btn-ghost btn-sm', onclick:
function() { api('/api/admin/role', 'POST', {
userId: u.id, role: nextRole
}).then(loadAdminPanel); } }, nextRole ===
'teacher' ? 'ت
 ب'));
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
}
aBox.appendChild(r);
});
var lBox =
document.getElementById('securityLogsBox');
lBox.innerHTML = '';
d.securityLogs.forEach(function(l) {
lBox.appendChild(el('div', { style:
'padding:6px 10px; border-radius:8px;
background:rgba(239,68,68,0.1); font
size:0.8rem;' }, '[' + l.time + '] ' + l.type +
': ' + l.detail));
});
}).catch(function() {});
}
function updateAnnouncement() {
api('/api/admin/settings', 'POST', {announcement:
document.getElementById('hostAnnouncementInput
).value }).then(function() { showToast('📢 ت
 
ث'); 
 
 
 
 
 
 });
}
function toggleGlobalAI() {
api('/api/admin/settings', 'POST', {
aiEnabled: !appState.settings.aiEnabled
}).then(function(d) { appState.settings =
d.settings; renderAll(); });
}
function addBook(e) {
e.preventDefault();
api('/api/admin/books', 'POST', { title:
document.getElementById('bookTitle').value,
subject:
document.getElementById('bookSubject').value,
url: document.getElementById('bookUrl').value
}).then(function(d) {
appState.books = d.books;
e.target.reset(); renderBooks(); showToast('📚
فة');
 
 
 
 
 
 
 
 
 
});
}
function addLink(e) {
e.preventDefault();
api('/api/admin/links', 'POST', { title:
document.getElementById('linkTitle').value,
url: document.getElementById('linkUrl').value
}).then(function(d) {
appState.links = d.links;e.target.reset(); renderLinks(); showToast('🔗
فة');
 
 
 
 
 
 
 
 
 
});
}
</script>
</body>
</html>`;
 app.get('/', (req, res) => {
res.type('html').send(HTML_PAGE);
});
 server.listen(PORT, () => {
console.log(`LEEV.study single-file secured
server running on port ${PORT}`);
})
