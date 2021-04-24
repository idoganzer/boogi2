---
title: April22
editable: false
---

<div align="center" style="background-color: #e5ecff; color: black">    <br/>    <div>DOC</div>    <h1>April22</h1>    <br/>  </div>

### Files Used:
📄 Userland/Libraries/LibCrypto/ASN1/DER.cpp

📄 Userland/Libraries/LibCrypto/ASN1/DER.h


<br/>

LibCrypo: Add an ASN.1/DER pretty-printer

It's much easier to debug things when we can actually *see* them :P

<br/>

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px; color: black">    📄 Userland/Libraries/LibCrypto/ASN1/DER.cpp  </div>

```cpp
⬜ 273        return {};
⬜ 274    }
⬜ 275    
🟩 276    void pretty_print(Decoder& decoder, OutputStream& stream, int indent)
🟩 277    {
🟩 278        while (!decoder.eof()) {
🟩 279            auto tag = decoder.peek();
🟩 280            if (tag.is_error()) {
🟩 281                dbgln("PrettyPrint error: {}", tag.error());
🟩 282                return;
🟩 283            }
🟩 284    
🟩 285            StringBuilder builder;
🟩 286            for (int i = 0; i < indent; ++i)
🟩 287                builder.append(' ');
🟩 288            builder.appendff("<{}> ", class_name(tag.value().class_));
🟩 289            if (tag.value().type == Type::Constructed) {
🟩 290                builder.appendff("[{}] {} ({})", type_name(tag.value().type), static_cast<u8>(tag.value().kind), kind_name(tag.value().kind));
🟩 291                if (auto error = decoder.enter(); error.has_value()) {
🟩 292                    dbgln("Constructed PrettyPrint error: {}", error.value());
🟩 293                    return;
🟩 294                }
🟩 295    
🟩 296                builder.append('\n');
🟩 297                stream.write(builder.string_view().bytes());
🟩 298    
🟩 299                pretty_print(decoder, stream, indent + 2);
🟩 300    
🟩 301                if (auto error = decoder.leave(); error.has_value()) {
🟩 302                    dbgln("Constructed PrettyPrint error: {}", error.value());
🟩 303                    return;
🟩 304                }
🟩 305    
🟩 306                continue;
🟩 307            } else {
🟩 308                if (tag.value().class_ != Class::Universal)
🟩 309                    builder.appendff("[{}] {} {}", type_name(tag.value().type), static_cast<u8>(tag.value().kind), kind_name(tag.value().kind));
🟩 310                else
🟩 311                    builder.appendff("[{}] {}", type_name(tag.value().type), kind_name(tag.value().kind));
🟩 312                switch (tag.value().kind) {
🟩 313                case Kind::Eol: {
🟩 314                    auto value = decoder.read<ReadonlyBytes>();
🟩 315                    if (value.is_error()) {
🟩 316                        dbgln("EOL PrettyPrint error: {}", value.error());
🟩 317                        return;
🟩 318                    }
🟩 319                    break;
🟩 320                }
🟩 321                case Kind::Boolean: {
🟩 322                    auto value = decoder.read<bool>();
🟩 323                    if (value.is_error()) {
🟩 324                        dbgln("Bool PrettyPrint error: {}", value.error());
🟩 325                        return;
🟩 326                    }
🟩 327                    builder.appendff(" {}", value.value());
🟩 328                    break;
🟩 329                }
🟩 330                case Kind::Integer: {
🟩 331                    auto value = decoder.read<ReadonlyBytes>();
🟩 332                    if (value.is_error()) {
🟩 333                        dbgln("Integer PrettyPrint error: {}", value.error());
🟩 334                        return;
🟩 335                    }
🟩 336                    builder.append(" 0x");
🟩 337                    for (auto ch : value.value())
🟩 338                        builder.appendff("{:0>2x}", ch);
🟩 339                    break;
🟩 340                }
🟩 341                case Kind::BitString: {
🟩 342                    auto value = decoder.read<const BitmapView>();
🟩 343                    if (value.is_error()) {
🟩 344                        dbgln("BitString PrettyPrint error: {}", value.error());
🟩 345                        return;
🟩 346                    }
🟩 347                    builder.append(" 0b");
🟩 348                    for (size_t i = 0; i < value.value().size(); ++i)
🟩 349                        builder.append(value.value().get(i) ? '1' : '0');
🟩 350                    break;
🟩 351                }
🟩 352                case Kind::OctetString: {
🟩 353                    auto value = decoder.read<StringView>();
🟩 354                    if (value.is_error()) {
🟩 355                        dbgln("OctetString PrettyPrint error: {}", value.error());
🟩 356                        return;
🟩 357                    }
🟩 358                    builder.append(" 0x");
🟩 359                    for (auto ch : value.value())
🟩 360                        builder.appendff("{:0>2x}", ch);
🟩 361                    break;
🟩 362                }
🟩 363                case Kind::Null: {
🟩 364                    auto value = decoder.read<decltype(nullptr)>();
🟩 365                    if (value.is_error()) {
🟩 366                        dbgln("Bool PrettyPrint error: {}", value.error());
🟩 367                        return;
🟩 368                    }
🟩 369                    break;
🟩 370                }
🟩 371                case Kind::ObjectIdentifier: {
🟩 372                    auto value = decoder.read<Vector<int>>();
🟩 373                    if (value.is_error()) {
🟩 374                        dbgln("Identifier PrettyPrint error: {}", value.error());
🟩 375                        return;
🟩 376                    }
🟩 377                    for (auto& id : value.value())
🟩 378                        builder.appendff(" {}", id);
🟩 379                    break;
🟩 380                }
🟩 381                case Kind::UTCTime:
🟩 382                case Kind::GeneralizedTime:
🟩 383                case Kind::IA5String:
🟩 384                case Kind::PrintableString: {
🟩 385                    auto value = decoder.read<StringView>();
🟩 386                    if (value.is_error()) {
🟩 387                        dbgln("String PrettyPrint error: {}", value.error());
🟩 388                        return;
🟩 389                    }
🟩 390                    builder.append(' ');
🟩 391                    builder.append(value.value());
🟩 392                    break;
🟩 393                }
🟩 394                case Kind::Utf8String: {
🟩 395                    auto value = decoder.read<Utf8View>();
🟩 396                    if (value.is_error()) {
🟩 397                        dbgln("UTF8 PrettyPrint error: {}", value.error());
🟩 398                        return;
🟩 399                    }
🟩 400                    builder.append(' ');
🟩 401                    for (auto cp : value.value())
🟩 402                        builder.append_code_point(cp);
🟩 403                    break;
🟩 404                }
🟩 405                case Kind::Sequence:
🟩 406                case Kind::Set:
🟩 407                    dbgln("Seq/Sequence PrettyPrint error: Unexpected Primtive");
🟩 408                    return;
🟩 409                }
🟩 410            }
🟩 411    
🟩 412            builder.append('\n');
🟩 413            stream.write(builder.string_view().bytes());
🟩 414        }
🟩 415    }
🟩 416    
⬜ 417    }
⬜ 418    
⬜ 419    void AK::Formatter<Crypto::ASN1::DecodeError>::format(FormatBuilder& fmtbuilder, Crypto::ASN1::DecodeError error)
```
<br/>

<div style="background: #e5ecff; padding: 10px 10px 10px 10px; border-bottom: 1px solid #c1c7d0; border-radius: 4px; color: black">    📄 Userland/Libraries/LibCrypto/ASN1/DER.h  </div>

```h
⬜ 220        Optional<Tag> m_current_tag;
⬜ 221    };
⬜ 222    
🟩 223    void pretty_print(Decoder&, OutputStream&, int indent = 0);
🟩 224    
⬜ 225    }
⬜ 226    
⬜ 227    template<>
```
<br/>

<br/><br/>

This file was generated by Swimm. [Click here to view it in the app](https://swimm.io/link?l=c3dpbW0lM0ElMkYlMkZyZXBvcyUyRmxySG5JSlhzMmRMUWhEUjhTRExKJTJGZG9jcyUyRnhmbGxnY1pzMmoxbzNJbGNNRFc1). Timestamp: 2021-04-22T15:24:51.933Z
