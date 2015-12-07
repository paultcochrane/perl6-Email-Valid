# Email::Valid
Email::Valid - WIP module to check validity of email addresses
## Synopsis
```perl6
use v6;
use Email::Valid;

my $email = Email::Valid.new(:simple(True));

if $email.validate("test@domain.tld") {
    say "test@domain.tld is valid";
}
say "Mailbox is: " ~ $email.parse('test@domain.tld')[0].Str;
# Mailbox is: test
```

## Description
This module validates if given email is valid.
It's a validation only for "most" popular email types.
It allows IDN domains ( 'xn--' )

## Methods
- validate( Str $email! --> Bool )
- parse( Str $email! --> Match )
- mx_validate( Str $email! --> Bool ) # Just check if domain has MX record
- extract( Str $text!, Bool :$matchs = False, Bool :$validate = False --> List )

## Examples
### Enable MX check
- 8.8.8.8 is the default DNS server
- 5 seconds is the default NS lookup timeout
```perl6
my $email = Email::Valid.new(:simple(False), :mx_check, :ns_server('8.8.8.8'), :ns_server_timeout(5) );

if !$email.validate("test@domain.tld") {
    say "test@domain.tld is NOT valid";
}
```

### Extract emails from text & validate them
```perl6
my $txt   = 'Some mails - <mail1@dont-exist.com,mail2@mail.com>'
my $email = Email::Valid.new(:simple(False), :mx_check );

$email.extract( $txt, :validate ) ; 
# (mail2@mail.com) because it has valid MX record
```

## TODO
- [x] Add MX Check
- [ ] Add "Hello" Callback verification
- [ ] Add TLD check ( create module Net::Domain::TLD )
- [ ] Add POD documentation
- [ ] Allow quoted mailboxes
- [ ] Allow ip addresses in domain part
- [ ] Fix ( Add ) IDN domain char limit
