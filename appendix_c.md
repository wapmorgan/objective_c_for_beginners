# Приложение С. Исходный код адресной книги

В справочных целях здесь приводятся в полном виде файлы секций interface и implementation для примера адресной книги, с которым мы работали в части II. Сюда включены определения классов AddressCard и AddressBook. Вы должны реализовать эти классы на своем компьютере; затем определения этих классов нужно расширить, чтобы сделать их более применимыми и усилить их возможности. Для нас это станет превосходным способом изучения языка и ознакомления с созданием программ, работой с классами и объектами, а также работой с Foundation framework.

## Файл секции interface для AddressCard
```
#import <Foundation/Foundation.h>
@interface AddressCard : NSObject <NSCopying, NSCoding>
{
NSString *name;
NSString *email;
}
@property (nonatomic, copy) NSString *name, *email;
-(void) setName: (NSString *) theName andEmail: (NSString *) theEmail;
-(void) retainName: (NSString *) theName andEmail: (NSString *) theEmail; -(NSComparisonResult) compareNames: (id) element;
-(void) print;
@end
```

## Файл секции interface для AddressBook
```
#import <Foundation/Foundation.h>
#import "AddressCard.h"
@interface AddressBook: NSObject <NSCopying, NSCoding>
{
NSString    *bookName;
NSMutableArray *book;
}
@property (nonatomic, copy) NSString *bookName;
@property (nonatomic, copy) NSMutableArray *book;
-(id) initWithName: (NSString *) name;
-(void) sort;
-(void) addCard: (AddressCard *) theCard;
-(void) removeCard: (AddressCard *) theCard;
-(int) entries;
-(void) list;
-(AddressCard *) lookup: (NSString *) theName;
-(void) dealloc;
@end
```

## Файл секции implementation для AddressCard
```
#import "AddressCard.h"
@implementation AddressCard @synthesize name, email;
-(void) setName: (NSString *) theName andEmail: (NSString *) theEmail
(
[seif setName: theName];
[self setEmail: theEmail];
}
// Сравнение двух имен из указанных адресных карточек -(NSComparisonResult) compareNames: (id) element {
return [name compare: [element name]];
}
-(void) print {
NSLog (@"============================");
NSLog (@"[                          ]");
NSLog (@"|   %-31s ]", [name UTF8String]);
NSLog (@"|   %-31s ]" , [email LFTFSString]);
NSLog (@"[                          ]");
NSLog (@"[                          ]");
NSLog (@"[                          ]");
NSLog (@"[     O      O             ]");
NSLog (@"============================");
}
-(AddressCard *) copyWithZone: (NSZone *) zone
{
AddressCard *newCard = [[AddressCard allocWithZone: zone] init];
[newCard retainName: name andEmail: email]; return newCard;
-(void) retainName: (NSString *) theName andEmail: (NSString *) theEmail
{
name = [theName retain]; email = [theEmail retain];
-(void) encodeWithCoder: (NSCoder *) encoder
{
[encoder encodeObject: name forKey: @"AddressCardName"]; [encoder encodeObject: email forKey: @"AddressCardEmail"];
-(id) initWithCoder: (NSCoder *) decoder
{
name = [[decoder decodeObjectForKey: @"AddressCardName"] retain]; email = [[decoder decodeObjectForKey: @"AddressCardEmail"] retain];
return self;
} -(void) dealloc
{
[name release];
[email release];
[super dealloc];
}
@end
```

## Файл секции implementation для AddressBook
```
#import "AddressBook.if
@implementation AddressBook
@synthesize book, bookName;
// задание имени адресной книги и пустой книги
-(id) initWithName: (NSString *) name{ self = [super init];
if (self) {
bookName = [[NSString alloc] initWithString: name]; book = [[NSMutableArray alloc] init];
}
return self;
}
-(void) sort
{
[book sortUsingSelector: @selector(compareNames:)];
}
-(void) addCard: (AddressCard *) theCard
{
[book addObject: theCard];
}
-(void) removeCard: (AddressCard *) theCard
{
[book removeObjectldenticalTo: theCard];
}
-(int) entries
{
return [book count];
}
-(void) list
{
NSLog (@"======== Contents of: %@ =========", bookName);
for (AddressCard "theCard in book )
NSLog (@"%-20s %-32s", [theCard.name UTF8String],
[theCard.email UTF8String]);
NSLog (@"============================================n);
}
// поиск адресной карточки по имени - требуется точное совпадение
-{AddressCard *) lookup: (NSString *) theName
{
for ( AddressCard *nextCard in book )
if ( [[nextCard name] caselnsensitiveCompare: theName] == NSOrderedSame ) return nextCard;
return nil;
}
-(void) dealloc
{
[bookName release];
[book release];
[super dealloc];
}
-(void) encodeWithCoder: (NSCoder *) encoder
{
[encoder encodeObject:bookName forKey: @"AddressBookBookName"];
[encoder encodeObject:book forKey: @"AddressBookBook"];
}
-(id) initWithCoder: (NSCoder *) decoder
(
bookName = [[decoder decodeObjectForKey: @"AddressBookBookName"] retain]; book = [[decoder decodeObjectForKey: @"AddressBookBook"] retain];
return self;
}
// Метод для протокола NSCopying
-(id) copyWithZone: (NSZone *) zone
{
AddressBook *newBook = [(self class] allocWithZone: zone];
[newBook initWithName: bookName];
[newBook setBook: book];
return newBook;
}
@end
```