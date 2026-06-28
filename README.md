## NextJS-API-JWT-Decode

### JWT Token Verification & Decoding
![](https://imgur.com/foUp1eV.png)

```bash
import { jwtVerify, SignJWT } from "jose";
import { NextRequest, NextResponse } from "next/server";



export async function GET(request:NextRequest) {
    
    const Key = new TextEncoder().encode(process.env.JWT_KEY);
    const payload = { email: "abc@gmail.com", user_id: "ABC123" };

    let token = await new SignJWT(payload)
        .setProtectedHeader({ alg: 'HS256' })
        .setIssuedAt()
        .setIssuer('https://localhost:3000')
        .setExpirationTime('2h')
        .sign(Key)

    return NextResponse.json(
        {token: token},
        {status: 200}
    )
}


export async function POST(request:NextRequest) {
    
    const jsonBody = await request.json();
    const Token = jsonBody['token'];

    const Key = new TextEncoder().encode(process.env.JWT_KEY);
    const decoded = await jwtVerify(Token, Key)

    return NextResponse.json(
        {message: decoded},
        {status: 200}
    )
}
```
---
