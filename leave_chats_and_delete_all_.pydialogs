import asyncio
from telethon.tl.functions.channels import LeaveChannelRequest
from telethon.tl.functions.messages import GetDialogsRequest
from telethon.tl.types import InputPeerEmpty
from telethon import TelegramClient


async def auth_client():
    api_id = 77777777
    api_hash = "hash"
    client = TelegramClient('main_session', api_id, api_hash)
    return client


async def delete_all_dialogs():
    client = await auth_client()
    count = 0
    await client.start()
    try:
        async for dialog in client.get_dialogs():
            count += 1
            leave = await client.delete_dialog(dialog.id, revoke=True)
            print(f"{count} - {leave}")
    except:
        pass


async def leave_all_groups():
    client = await auth_client()
    me = await client.get_me()
    print(me)
    chats = []
    last_date = None
    chunk_size = 200
    groups = []

    result = await client(GetDialogsRequest(
        offset_date=last_date,
        offset_id=0,
        offset_peer=InputPeerEmpty(),
        limit=chunk_size,
        hash=0
    ))
    chats.extend(result.chats)

    for chat in chats:
        try:
            if chat == True:
                groups.append(chat)
        except:
            continue

    leave_chat = []
    for all_groups in groups:
        if all_groups.username != None:
            leave_chat.append(all_groups.username)
    for leave_chats in range(len(leave_chat)):
        result = await client(LeaveChannelRequest(channel=leave_chat[leave_chats]))
        print(f'{result.id}, {result.username} - аккаунт вышел из группы...')


async def main() -> None:
    delete_dialog = await delete_all_dialogs()
    leave_groups = await leave_all_groups()
    tasks = [delete_dialog, leave_groups]
    await asyncio.gather(*tasks)


if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
