---
layout: post
title: Parsing in Lua
---

Traditional recursive-descent parsing. First splits input stream into tokens. It exposes two major functions:

    void luaX_next(LexState *s);
    void luaX_lookahead(LexState *s);
    
The first assigns the text token in the stream to the state's current token. If there is already a lookahead token present,
it reuses that. The second lexes the next token into the lookahead token slot. (This explanation is awful.)

The actual lexing is implemented as a giant switch statement on the first character.

These three functions control the actual parsing:


    static int testnext (LexState *ls, int c) {
        if (ls->t.token == c) {
            luaX_next(ls);
            return 1;
        }
        else return 0;
    }
    
    static void check (LexState *ls, int c) {
        if (ls->t.token != c)
            error_expected(ls, c);
    }
     
    static void checknext (LexState *ls, int c) {
        check(ls, c);
        luaX_next(ls);
    }
